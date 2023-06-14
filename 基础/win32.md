```cpp
//#include<Windows.h>
#include "windows.h"
#include<windowsx.h>
#include<tchar.h>

/*
1.创建窗口类
2.注册窗口类
3.实例化窗口类
4.显示窗口类
5.让窗口一直显示（处理窗口消息）

*/

void print(LPCWCHAR format,...)
{

  // 这下面5行都是为了能实时输出坐标信息而产生的代码。
    WCHAR wchar_buff[100]{0};
    va_list arglist;
    va_start(arglist, format);
    wvsprintfW(wchar_buff, format, arglist);
    va_end(arglist);
    OutputDebugStringW(wchar_buff);  // 单独输出这行也可以，但是这样就不能输出坐标。
}

LRESULT CALLBACK MainWndProc(
    HWND hwnd,        // handle to window
    UINT uMsg,        // message identifier
    WPARAM wParam,    // first message parameter
    LPARAM lParam
) 
{

    switch (uMsg)
    {
    case WM_CREATE:
        return 0;

    case WM_PAINT:
        {
            // 1、标签绘画结构
            PAINTSTRUCT ps{};
            // 2、开始绘画
            auto hdc = BeginPaint(hwnd, &ps);


            //
            FillRect(hdc, &ps.rcPaint, (HBRUSH)(COLOR_WINDOW + 1));

            EndPaint(hwnd, &ps);
        }
        return 0;

    case WM_MOUSEMOVE:
        WORD x = GET_X_LPARAM(lParam);
        WORD y = GET_Y_LPARAM(lParam);
        print(L"鼠标移动了\t , 坐标 %d, 坐标 %d\n", x, y);
        //print(L"鼠标移动了\t"  + x +  y);
        break;
    }
    return DefWindowProc(hwnd, uMsg, wParam, lParam);
}

int WINAPI WinMain(
    _In_ HINSTANCE hInstance,
    _In_opt_ HINSTANCE hPrevInstance,
    _In_ LPSTR lpCmdLine,
    _In_ int nShowCmd
)
{
    const wchar_t className[]{ L"hello world" };

    // 创建窗口类
    WNDCLASS WS{};
    WS.lpfnWndProc = MainWndProc;
    WS.lpszClassName = className;
    WS.hInstance = hInstance;

    // 注册窗口
    RegisterClass(&WS);

    // 实例化窗口
    HWND hwnd = CreateWindowExW(
        _In_ WS_EX_CONTEXTHELP,
        _In_opt_ className,
        _In_opt_ L"LPCWSTR lpWindowName",
        _In_ WS_OVERLAPPEDWINDOW,
        _In_ CW_USEDEFAULT,
        _In_ CW_USEDEFAULT,
        _In_ CW_USEDEFAULT,
        _In_ CW_USEDEFAULT,
        _In_opt_ NULL,
        _In_opt_ NULL,
        _In_opt_ hInstance,
        _In_opt_ NULL
    );

    if (hwnd == 0)
    {
        return 0;
    }

    ShowWindow(hwnd, nShowCmd);

    MSG msg{};
    while (GetMessage(&msg, hwnd, 0,0) > 0)
    {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }

    return 0;
}
```
