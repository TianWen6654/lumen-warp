# Windows x64 构建说明（待独立环境复验）

1. 获取 `Heartcoolman/warp-cn` 提交 `a714a1ad833053db6f190f7fff38ecccc9fcd6ae` 的源码及其 Git LFS 资源。此版本社区仓库的 LFS 资源在 Warp 上游可获取；完整源码归档也可作为输入。
2. 在源码目录执行 `git apply --check <补丁文件路径>`，再执行 `git apply <补丁文件路径>`。本项目补丁仍在核验，尚未上传。
3. 按源码仓库的构建要求安装 Rust 工具链、Visual Studio C++ Build Tools、Windows SDK、Git、CMake 和 protoc，并在 x64 开发者环境中运行。
4. 以源码根目录为当前目录，执行：

   ```powershell
   cargo build --locked --release --jobs 2 --config 'profile.release.package.warp.opt-level=0' --config 'profile.release.package.warp.debug=0' --config 'profile.release.package.warp.codegen-units=256' --bin warp-oss --features gui
   ```

5. 用 Rust 的 `--remap-path-prefix` 映射所有个人路径；Windows C/C++ 依赖也需要 MSVC 的 `/experimental:deterministic /pathmap:原路径=替代路径`。检查生成的程序、调试目录和安装包中是否仍含个人信息；路径重映射后应在独立环境测试。
6. 配置 Inno Setup 的安装内容，包含运行库、资源、许可证、对应源码和公开版更新检查脚本。安装包模板处于测试阶段，尚未上传，需按最终产物调整。

Windows 解压源码归档时，`.claude/skills` 符号链接可能被保存为文本文件；它不应被误认为实际目录。构建材料必须重新核验，不应依赖本机 Codex 缓存路径。

