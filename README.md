# OpenWRT-CI (ZN-M2 纯有线精简版)

基于 [VIKINGYFY/OpenWRT-CI](https://github.com/VIKINGYFY/OpenWRT-CI) 定制的云编译仓库，当前专注构建 **兆能 ZN-M2（`qualcommax/ipq60xx`）纯有线精简固件**，基于 [VIKINGYFY/immortalwrt](https://github.com/VIKINGYFY/immortalwrt.git)（`owrt` 分支）。

## 固件特性（`ZN-M2-WIFI-NO`）

- **单机型纯有线（`WIFI-NO`）**：使用 `nowifi.dtsi` 调整 Q6 内存，移除 `ath`/`ath11k` 无线驱动固件及 NSS Wi-Fi offload，有线直走 `qca_edma` / `qca_ppe` + eBPF。
- **eBPF / BTF / honk 就绪**：完整开启 `BPF`、`BPF_JIT`、`CGROUP_BPF`、`DEBUG_INFO_BTF`、`kmod-sched-bpf`、`kmod-nft-queue`、`kmod-veth`、`bpftool-minimal` 等内核与运行时依赖，兼容原生 `honk-core` 与 OpenWrt `honk` apk（不内置用户态代理程序）。
- **网络与内存优化**：
  - 启用 `fullconenat-sonic` + `BBR` + `fq`（关闭 Flow Offload 防止绕过 TC）
  - 开启 `SKB_RECYCLER` 与 `SKB_RECYCLER_MULTI_CPU` 提升包转发效率
  - 启用 `ZRAM + LZ4` 内存压缩
  - 扩大 `qualcommax` 内核分区上限至 `12288k` 以容纳 BTF/debuginfo
- **极限精简**：裁剪 USB/存储/自动挂载、音频、UPnP、WOL、`attendedsysupgrade`、`coremark` 等冗余组件。

## 默认访问信息

- **管理地址**：`192.168.10.1`
- **登录密码**：无
- **默认主题**：`vantage`（简体中文）
- **无线状态**：纯有线版本（已禁用 Wi-Fi）

## 仓库目录说明

- `.github/workflows/`：CI 工作流配置
  - `ZN-M2.yml`：ZN-M2 主编译工作流（支持 `workflow_dispatch` 手动触发，可传 `PACKAGE` 追加插件或勾选 `TEST` 仅输出配置）
  - `WRT-CORE.yml`：通用编译核心流程
  - `Auto-Clean.yml`：清理旧 Release 与 Workflow 记录（当前定时任务已关闭，仅手动触发；运行完成后会自动触发 `ZN-M2` 编译）
  - `Cache-Clean.yml`：清理 GitHub Actions 缓存
- `Config/`：设备配置文件（`ZN-M2-WIFI-NO.txt`）
- `Scripts/`：自定义构建脚本（`Handles.sh`、`Packages.sh`、`Settings.sh`）

## 上游与相关参考

- 上游源码：[VIKINGYFY/immortalwrt](https://github.com/VIKINGYFY/immortalwrt.git)（`owrt` 分支）
- 官方源码：[immortalwrt/immortalwrt](https://github.com/immortalwrt/immortalwrt.git)
- 上游 CI 模板：[VIKINGYFY/OpenWRT-CI](https://github.com/VIKINGYFY/OpenWRT-CI)
- 自用插件源：[VIKINGYFY/packages](https://github.com/VIKINGYFY/packages.git)
- 高通 U-Boot 参考：[uboot-qsdk12.5-build](https://github.com/chenxin527/uboot-qsdk12.5-build.git) / [u-boot-2016](https://github.com/1980490718/u-boot-2016.git)
