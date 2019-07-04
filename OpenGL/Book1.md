# Windows中基于外部窗口句柄创建OpenGL 3.3 环境
---
## 主要流程

### 1. 获取WGL函数指针

#### 创建一个临时窗口
```
WNDCLASSEXW wc;

ZeroMemory(&wc, sizeof(WNDCLASSEXW));
wc.cbSize = sizeof(wc);
wc.style = CS_HREDRAW | CS_VREDRAW | CS_OWNDC;
wc.lpfnWndProc = (WNDPROC)[](HWND hWnd, UINT uMsg, WPARAM wParam, LPARAM lParam)->LRESULT {
    return DefWindowProcW(hWnd, uMsg, wParam, lParam);
};
wc.hInstance = GetModuleHandleW(NULL);
wc.hCursor = LoadCursorW(NULL, IDC_ARROW);
wc.lpszClassName = HELPWINDOWCLASSNAME;

if (!RegisterClassExW(&wc)) {
    return nullptr;
}

HWND window = CreateWindowExW(WS_EX_OVERLAPPEDWINDOW,
                              L"CJieWGL",
                              L"CJieWGL helper window",
                              WS_CLIPSIBLINGS | WS_CLIPCHILDREN,
                              0, 0, 1, 1,
                              HWND_MESSAGE, nullptr,
                              GetModuleHandleW(nullptr),
                              nullptr);
if (!window) {
    return nullptr;
}

ShowWindow(window, SW_HIDE);
```
#### 基于临时窗口，获取wgl的HGLRC
```
// 创建初始化环境窗口
HWND hWnd = createHelperWindow();
if (nullptr == hWnd) {
    return EXCEPTION;
}

// 获取DC
HDC hDC = ::GetDC(hWnd);

PIXELFORMATDESCRIPTOR pfd = { 0 };
int result = ::SetPixelFormat(hDC, 1, &pfd);
if (result != 1) {
    ReleaseDC(hWnd, hDC);
    DestroyWindow(hWnd);
    return EXCEPTION;
}

// 创建RC
HGLRC hRC = wglCreateContext(hDC);
if (nullptr == hRC) {
    ReleaseDC(hWnd, hDC);
    DestroyWindow(hWnd);
    return EXCEPTION;
}
```
#### 将临时窗口的HDC和HGLRC进行绑定
```
// 创建RC
HGLRC hRC = wglCreateContext(hDC);
if (nullptr == hRC) {
    ReleaseDC(hWnd, hDC);
    DestroyWindow(hWnd);
    return EXCEPTION;
}

//选择RC作为当前线程的RC
BOOL b = wglMakeCurrent(hDC, hRC);
if (!b) {
    wglDeleteContext(hRC);
    ReleaseDC(hWnd, hDC);
    DestroyWindow(hWnd);
    return EXCEPTION;
}
```
#### 获取wgl函数指针地址，保存待使用的函数指针
```
bool r = false;
r = ((wglCreateContextAttribsARB = (PFNWGLCREATECONTEXTATTRIBSARBPROC)wglGetProcAddress("wglCreateContextAttribsARB")) == NULL) || r;
r = ((wglChoosePixelFormatARB = (PFNWGLCHOOSEPIXELFORMATARBPROC)wglGetProcAddress("wglChoosePixelFormatARB")) == NULL) || r;
return !r;
```
#### 释放所有临时窗口的资源和HGLRC
```
// 释放初始化环境
wglMakeCurrent(hDC, nullptr);
wglDeleteContext(hRC);
ReleaseDC(hWnd, hDC);
DestroyWindow(hWnd);

// 反注册 初始化环境窗口
UnregisterClass(HELPWINDOWCLASSNAME, GetModuleHandleW(NULL));
```
### 2. 将OpenGL环境和外部窗口句柄关联

#### 获取外部窗口句柄的HDC
```
_hDC = ::GetDC(_hWnd);
if (nullptr == _hDC) {
    freeGLEx();
    return EXCEPTION;
}
```
#### 使用WGL函数指针，选择HDC支持的像素格式
```
int pixelFormat[1] = { 0 };
unsigned int formatCount = 0;
int result = wglChoosePixelFormatARB(_hDC, _attrListInt, NULL, 1, pixelFormat, &formatCount);
if (result != 1 || 0 == formatCount) {
    freeGLEx();
    return EXCEPTION;
}
```
#### 设置HDC的像素格式
```
PIXELFORMATDESCRIPTOR pixelFormatDescriptor = { 0 };
result = ::SetPixelFormat(_hDC, pixelFormat[0], &pixelFormatDescriptor);
if (result != 1) {
    DLOG(ERROR) << "SetPixelFormat Error = " << GetLastError();
    freeGLEx();
    return PARAMERROR;
}
```
#### 使用WGL函数指针创建OpenGL3.3以上的环境HRC
```
// init thread HRC
int attributeList[5] = { 0 };
// Set the 3.3 version of OpenGL in the attribute list.
attributeList[0] = WGL_CONTEXT_MAJOR_VERSION_ARB;
attributeList[1] = 3;
attributeList[2] = WGL_CONTEXT_MINOR_VERSION_ARB;
attributeList[3] = 3;

// Create a OpenGL 3.3 rendering context.
_hRC = wglCreateContextAttribsARB(_hDC, 0, attributeList);
if (_hRC == NULL) {
    freeGLEx();
    return EXCEPTION;
}
```
#### 绑定当前窗口的HDC和HRC，成功后OpenGL的绘制就显示在外部给定的窗口上
```
// Set the rendering context to active.
result = wglMakeCurrent(_hDC, _hRC);
if (result != 1) {
    freeGLEx();
    return EXCEPTION;
}
```

## 注意

1. windows 窗口的像素格式只能设置一次，需要一个临时的窗口获取WGL函数指针。获取WGL函数指针必须先有一个OpenGL的环境。所以使用临时窗口创建一个OpenGL1.0环境，获取到WGL指针后释放临时的窗口和OpenGL环境。使用WGL函数指针创建OpenGL3.3环境
1. HRC是和线程绑定的，创建好之后切换线程绘制的时候会出现显示错误。多个窗口相同线程情况使用相同的HRC绑定不同的HDC。