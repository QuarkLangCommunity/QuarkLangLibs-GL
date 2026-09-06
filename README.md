# QuarkLangLibs-GL

**QuarkLang 官方库（gl）——OpenGL 图形 API 绑定，官方认证。**

`gl.qk` 通过语言级 **`library` 系统库绑定**直接声明 OpenGL 导出符号，
运行时跨系统加载（Linux `libGL.so*` / macOS `libGL.dylib` / Windows `opengl32.dll`）
并经 **libffi** 调用——完全对应系统图形驱动。

## 使用方法

把 `gl.qk` 放到源码目录并编写（需要宿主平台实现 OpenGL 上下文与窗口）：

```qk
import "gl";

fn main(io IOStream) {
    gl.ClearColor(0.2, 0.3, 0.4, 1.0);
    gl.Clear(glc::COLOR_BUFFER_BIT() + glc::DEPTH_BUFFER_BIT());

    gl.Begin(glc::TRIANGLES());
    gl.Color(1.0, 0.0, 0.0, 1.0);
    gl.Vertex(-0.5, -0.5);
    gl.Color(0.0, 1.0, 0.0, 1.0);
    gl.Vertex(0.5, -0.5);
    gl.Color(0.0, 0.0, 1.0, 1.0);
    gl.Vertex(0.0, 0.5);
    gl.End();
    gl.Flush();
}
```

## 声明集 API

| 函数 | 签名 | 对应 GL |
|---|---|---|
| `ClearColor` | `(f32,f32,f32,f32) void` | glClearColor |
| `Clear` | `(int) void` | glClear（mask 常量见 `glc::` 空间） |
| `Viewport` | `(int,int,int,int) void` | glViewport |
| `Enable`/`Disable` | `(int) void` | glEnable/glDisable |
| `Begin`/`End`/`Vertex`/`Color` | 立即模式 | glBegin/glEnd/glVertex2f/glColor4f |
| `Flush`/`Finish` | `() void` | glFlush/glFinish |
| `GetString` | `(int) String` | glGetString（VERSION/VENDOR/RENDERER） |
| `GetError` | `() int` | glGetError |

常量在 `glc::` 空间（`glc::COLOR_BUFFER_BIT()` 等）。

## 认证信息

- 库名：`gl`（`import "gl"`）—— `library gl` 声明 + `glc` 常量空间
- 语言版本：QuarkLang v0.2（`library` FFI / `space` / 泛型体系）
- 运行时版本：见主仓库 `engineVersion`
- 认证方：QuarkLang 官方项目

> 说明：`gl.qk` 是**声明集（认证）**——实际调用由运行时 FFI 完成；
> 窗口/上下文（GLFW/WGL/GLX）由宿主负责，本库只管 OpenGL 调用面。
