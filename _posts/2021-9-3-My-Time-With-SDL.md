## 使用SDL的注意点
```
#define main SDL_main

// And then
extern C_LINKAGE int SDL_main(int argc, char *argv[]);

```
抽象 Windows 上的不同入口点 (winmain) 和其他平台上的传统入口点 (int main)。 它简化了许多原本需要 ifdef 或单独源文件的代码。 这是一种方便的机制，如果您愿意，可以手动完成