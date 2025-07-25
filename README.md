
# BC3 - C bindings for C3

BC3 - is a repository which contains a bunch of automatically generated bindings, powered by [bindgen.c3l](https://github.com/vssukharev/bindgen.c3l).

## Setup

To install bindings for library **foo**, simply copy **foo.c3l** located in **libs** to your project and set **foo** as dependenncy.

## Generating bindings

First of all, install **libclang** required by [bindgen.c3l](https://github.com/vssukharev/bindgen.c3l), then simply run `c3c run glfw` as an examples. The following bindings are currently supported:
- GLFW
- Vulkan
- LLVM (Nearly complete)
- Python (Mostly incomplete)

## Creating your own bindings

For that you need to do 3 steps:

1. Add required headers (and dependent headers also) to `headers` directory
2. Create `libs/yourlib.c3l` directory with according `manifest.json` inside of it
3. Create binding generator itself inside of `generators/` directory. For example, to create bindings for glfw, you need to:
  - Create `generators/glfw.c3`
  - Add target `glfw` to `project.json`:
  ```json
  {
    "targets" : {
      "glfw" : {
        "sources" : [ "generators/glfw.c3" ],
        "type" : "executable",
      },
    }
  }
  ```
  - Specify `module gen;` on top of `generators/glfw.c3`
  - Write a function named the same as your created file, `glfw` on our case, which takes two parameters: `String src` and `String out` - source and output file names respectively. It must perform the generation itself. See already written bindings and look at `bindgen/bindgen.c3l/bindgen.c3` for provided **bindgen** API
  - Call this function inside of `generators/main.c3` entry point via `@generate` macro, which takes three paramters: 1st - `main` arguments, 2nd - header file name (respectively to `headers` directory) and 3rd - library name. As you can see the third parameter defines library name, function name and interface file name at the same time.
4. Run `c3c run yourlibrary` to check it out

