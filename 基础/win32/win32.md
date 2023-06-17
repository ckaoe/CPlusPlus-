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



----

```CPP
#include<Windows.h>
#include<windowsx.h>
#include<CommCtrl.h>
#include<iostream>



LRESULT CALLBACK MainWndProc(
    HWND hwnd,        // 父窗口的句柄
    UINT uMsg,        // 消息
    WPARAM wParam,    // 消息的ID
    LPARAM lParam)    // 消息的句柄
{
    HWND hwndButton{};

    switch (uMsg)
    {

    case WM_PAINT:
    {
        PAINTSTRUCT ps;
        HDC hdc = BeginPaint(hwnd, &ps);

        // All painting occurs here, between BeginPaint and EndPaint.
        FillRect(hdc, &ps.rcPaint, (HBRUSH)(COLOR_WINDOW + 1));
        EndPaint(hwnd, &ps);
    }
    return 0;

    case WM_CREATE:
        // 这里是GetWindowLongPtr是检索窗口的实例句柄
        CreateWindow(L"BUTTON", L"ok", WS_VISIBLE | WS_CHILD, 10, 10, 100, 40, hwnd, (HMENU)0x100, (HINSTANCE)GetWindowLongPtr(hwnd, GWLP_HINSTANCE), NULL);
        // 使用菜单项作为id
        CreateWindow(L"EDIT", L"hello world", WS_BORDER|WS_VISIBLE | WS_CHILD, 10, 60, 300, 40, hwnd, (HMENU)0x101, (HINSTANCE)GetWindowLongPtr(hwnd, GWLP_HINSTANCE), NULL);
        CreateWindow(L"BUTTON", L"获取句柄", WS_VISIBLE | WS_CHILD, 10, 120, 100, 40, hwnd, (HMENU)0x102, (HINSTANCE)GetWindowLongPtr(hwnd, GWLP_HINSTANCE), NULL);

        

        
        
        return 0;

    case WM_COMMAND:
        auto buttonid = LOWORD(wParam) ;
        switch (buttonid)
        {
        case 0x100:
        {
            /*MoveWindow((HWND)lParam, 80, 80,100, 40, 1);*/
            wchar_t buff[100]{ 0 };
            //GetDlgItem（）返回指定对话框中的句柄
            HWND hwnds = GetDlgItem(hwnd, 0x101);
            /*
            * 返回窗口的文本。
            * hwnd：包含文本的窗口或者句柄。即：要获取的内容的窗口的句柄，所以需要使用GetDlgItem（）才能获取。
            * lpString：将接受文本的缓冲区。即：创建一个变量，用具接受返回的文本。
            * nMaxCount：要返回到缓冲区的最大字符数。即：最大的字符串数。
            */ 
            GetWindowTextW(hwnds, buff, 100);  
            // 显示返回的信息
            MessageBoxW(hwnds, buff, L"hello", MB_OK);
        }

        case 0x102:
        {
            HWND hwnda = FindWindow(NULL, L"无标题 - Notepad");
            SetParent((HWND)lParam, hwnda);

        }
            
        default:
            break;
        }


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
    const wchar_t className[] = L"hello world";

    WNDCLASS ws{};
    ws.lpfnWndProc = MainWndProc;
    ws.lpszClassName = className;

    RegisterClass(&ws);

    HWND hwnd = CreateWindowExW
    (
        _In_ 0,
        _In_opt_ className,
        _In_opt_ L"WindowName",
        _In_ WS_OVERLAPPEDWINDOW,
        _In_ CW_USEDEFAULT,
        _In_ CW_USEDEFAULT,
        _In_ 500,
        _In_ 500,
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
    while (GetMessage(&msg, hwnd, 0, 0) > 0)
    {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }

    return 0;
}

```
