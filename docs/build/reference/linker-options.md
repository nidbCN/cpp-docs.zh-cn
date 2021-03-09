---
title: MSVC 链接器选项
description: Microsoft LINK 链接器支持的选项的列表。
ms.date: 09/01/2020
f1_keywords:
- link
helpviewer_keywords:
- linker [C++]
- linker [C++], options listed
- libraries [C++], linking to COFF
- LINK tool [C++], linker options
ms.assetid: c1d51b8a-bd23-416d-81e4-900e02b2c129
ms.openlocfilehash: e426c085acdd0d7bcfeb00b17dcf733a9b856993
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465309"
---
# <a name="linker-options"></a>链接器选项

LINK.exe 将通用对象文件格式 (COFF) 对象文件和库链接起来，以创建可执行 (.exe) 文件或动态链接库 (DLL)。

下表列出了 LINK.exe 的选项。 有关 LINK 的详细信息，请参阅：

- [编译器控制的链接选项](compiler-controlled-link-options.md)

- [链接输入文件](link-input-files.md)

- [链接输出](link-output.md)

- [保留字](reserved-words.md)

在命令行上，链接器选项不区分大小写;例如， `/base` 和 `/BASE` 的含义相同。 有关如何在命令行或 Visual Studio 中指定每个选项的详细信息，请参阅适用于该选项的文档。

可以使用 [注释](../../preprocessor/comment-c-cpp.md) 杂注指定一些链接器选项。

## <a name="linker-options-listed-alphabetically"></a>按字母顺序列出的链接器选项

|选项|目标|
|------------|-------------|
|[@](at-specify-a-linker-response-file.md)|指定响应文件。|
|[/ALIGN](align-section-alignment.md)|指定每一节的对齐方式。|
|[/ALLOWBIND](allowbind-prevent-dll-binding.md)|指定 DLL 不能绑定。|
|[/ALLOWISOLATION](allowisolation-manifest-lookup.md)|指定清单查找的行为。|
|[/APPCONTAINER](appcontainer-windows-store-app.md)|指定应用是否必须在 appcontainer 进程环境中运行。|
|[/ASSEMBLYDEBUG](assemblydebug-add-debuggableattribute.md)|将 <xref:System.Diagnostics.DebuggableAttribute> 添加到托管映像中。|
|[/ASSEMBLYLINKRESOURCE](assemblylinkresource-link-to-dotnet-framework-resource.md)|创建指向托管资源的链接。|
|[/ASSEMBLYMODULE](assemblymodule-add-a-msil-module-to-the-assembly.md)|指定应将 Microsoft 中间语言 (MSIL) 模块导入到程序集中。|
|[/ASSEMBLYRESOURCE](assemblyresource-embed-a-managed-resource.md)|将托管资源文件嵌入程序集。|
|[/BASE](base-base-address.md)|为程序设置基址。|
|[/CETCOMPAT](cetcompat.md)|将二进制文件标记为 CET 影子堆栈兼容。|
|[/CGTHREADS](cgthreads-compiler-threads.md)|设置 cl.exe 线程数以在指定链接时代码生成后用于优化和代码生成。|
|[/CLRIMAGETYPE](clrimagetype-specify-type-of-clr-image.md)|设置 CLR 映像的类型（IJW、纯或安全）。|
|[/CLRSUPPORTLASTERROR](clrsupportlasterror-preserve-last-error-code-for-pinvoke-calls.md)|保留通过 P/Invoke 机制调用的函数的上一个错误代码。|
|[/CLRTHREADATTRIBUTE](clrthreadattribute-set-clr-thread-attribute.md)|指定要应用于 CLR 程序入口点的线程特性。|
|[/CLRUNMANAGEDCODECHECK](clrunmanagedcodecheck-add-suppressunmanagedcodesecurityattribute.md)|指定链接器是否将 SuppressUnmanagedCodeSecurity 特性应用于链接器生成的、从托管代码调用到本机 DLL 中的 PInvoke 存根。|
|[/DEBUG](debug-generate-debug-info.md)|创建调试信息。|
|[/DEBUGTYPE](debugtype-debug-info-options.md)|指定要包括在调试信息中的数据。|
|[/DEF](def-specify-module-definition-file.md)|将模块定义 (.def) 文件传递到链接器。|
|[/DEFAULTLIB](defaultlib-specify-default-library.md)|在解析外部引用时搜索指定的库。|
|[/DELAY](delay-delay-load-import-settings.md)|控制 DLL 的延迟加载。|
|[/DELAYLOAD](delayload-delay-load-import.md)|导致延迟加载指定的 DLL。|
|[/DELAYSIGN](delaysign-partially-sign-an-assembly.md)|对程序集进行部分签名。|
|[/DEPENDENTLOADFLAG](dependentloadflag.md)|设置依赖 DLL 加载的默认标志。|
|[/DLL](dll-build-a-dll.md)|生成 DLL。|
|[/DRIVER](driver-windows-nt-kernel-mode-driver.md)|创建内核模式驱动程序。|
|[/DYNAMICBASE](dynamicbase-use-address-space-layout-randomization.md)|指定是否在加载时使用地址空间布局随机化 (ASLR) 功能生成变基的可执行映像。|
|[/ENTRY](entry-entry-point-symbol.md)|设置起始地址。|
|[/ERRORREPORT](errorreport-report-internal-linker-errors.md)| 已弃用。 错误报告由 [Windows 错误报告 (WER) ](/windows/win32/wer/windows-error-reporting) 设置控制。 |
|[/EXPORT](export-exports-a-function.md)|导出函数。|
|[/FILEALIGN](filealign.md)|将输出文件中的部分与指定值的倍数对齐。|
|[/FIXED](fixed-fixed-base-address.md)|创建只能在其首选基址加载的程序。|
|[/FORCE](force-force-file-output.md)|强制完成链接，即使符号无法解析或已定义多次。|
|[/FUNCTIONPADMIN](functionpadmin-create-hotpatchable-image.md)|创建可进行热修补的映像。|
|[/GENPROFILE、/FASTGENPROFILE](genprofile-fastgenprofile-generate-profiling-instrumented-build.md)|这两个选项均 *`.pgd`* 通过链接器指定文件的生成，以支持按配置文件优化 (PGO) 。 /GENPROFILE 和 /FASTGENPROFILE 使用不同的默认参数。|
|[/GUARD](guard-enable-guard-checks.md)|启用控制流防护保护。|
|[/HEAP](heap-set-heap-size.md)|设置堆的大小（以字节为单位）。|
|[/HIGHENTROPYVA](highentropyva-support-64-bit-aslr.md)|指定对高熵 64 位地址空间布局随机化 (ASLR) 的支持。|
|[/IDLOUT](idlout-name-midl-output-files.md)|指定 *`.idl`* 文件和其他 MIDL 输出文件的名称。|
|[/IGNORE](ignore-ignore-specific-warnings.md)|禁止显示指定链接器警告的输出。|
|[/IGNOREIDL](ignoreidl-don-t-process-attributes-into-midl.md)|防止将特性信息处理到文件中 *`.idl`* 。|
|[/IMPLIB](implib-name-import-library.md)|重写默认的导入库名。|
|[/INCLUDE](include-force-symbol-references.md)|强制符号引用。|
|[/INCREMENTAL](incremental-link-incrementally.md)|控制增量链接。|
|[/INFERASANLIBS](inferasanlibs.md)|使用推断的 sanitizer 库。|
|[/INTEGRITYCHECK](integritycheck-require-signature-check.md)|指定模块需要在加载时进行签名检查。|
|[/KEYCONTAINER](keycontainer-specify-a-key-container-to-sign-an-assembly.md)|指定用来对程序集进行签名的密钥容器。|
|[/KEYFILE](keyfile-specify-key-or-key-pair-to-sign-an-assembly.md)|指定用来对程序集进行签名的密钥或密钥对。|
|[/LARGEADDRESSAWARE](largeaddressaware-handle-large-addresses.md)|通知编译器应用程序支持大于 2 GB 的地址|
|[/LIBPATH](libpath-additional-libpath.md)|指定要在环境库路径之前搜索的路径。|
|[/LINKREPRO](linkrepro.md)|指定要在其中生成链接重现项目的路径。|
|[/LINKREPROTARGET](linkreprotarget.md)|仅在生成指定的目标时才生成链接重现。<sup>16.1</sup>|
|[/LTCG](ltcg-link-time-code-generation.md)|指定链接时间代码生成。|
|[/MACHINE](machine-specify-target-platform.md)|指定目标平台。|
|[/MANIFEST](manifest-create-side-by-side-assembly-manifest.md)|创建并行清单文件，也可以选择将其嵌入二进制文件。|
|[/MANIFESTDEPENDENCY](manifestdependency-specify-manifest-dependencies.md)|指定 \<dependentAssembly> 清单文件中的部分。|
|[/MANIFESTFILE](manifestfile-name-manifest-file.md)|更改清单文件的默认名称。|
|[/MANIFESTINPUT](manifestinput-specify-manifest-input.md)|指定链接器要进行处理并嵌入二进制文件的清单输入文件。 可以多次使用此选项以指定多个清单输入文件。|
|[/MANIFESTUAC](manifestuac-embeds-uac-information-in-manifest.md)|指定是否将用户帐户控制 (UAC) 信息嵌入到程序清单中。|
|[/MAP](map-generate-mapfile.md)|创建映射文件。|
|[/MAPINFO](mapinfo-include-information-in-mapfile.md)|包括映射文件中的指定信息。|
|[/MERGE](merge-combine-sections.md)|合并节。|
|[/MIDL](midl-specify-midl-command-line-options.md)|指定 MIDL 命令行选项。|
|[/NATVIS](natvis-add-natvis-to-pdb.md)|将调试器可视化工具从 Natvis 文件添加到 (PDB) 的程序数据库。|
|[/NOASSEMBLY](noassembly-create-a-msil-module.md)|取消创建 .NET Framework 程序集。|
|[/NODEFAULTLIB](nodefaultlib-ignore-libraries.md)|在解析外部引用时忽略所有（或指定的）默认库。|
|[/NOENTRY](noentry-no-entry-point.md)|创建纯资源 DLL。|
|[/NOLOGO](nologo-suppress-startup-banner-linker.md)|取消显示启动版权标志。|
|[/NXCOMPAT](nxcompat-compatible-with-data-execution-prevention.md)|将可执行文件标记为经验证与 Windows 数据执行保护功能兼容。|
|[/OPT](opt-optimizations.md)|控制 LINK 优化。|
|[/ORDER](order-put-functions-in-order.md)|按预先确定的顺序将 COMDAT 放置到映像中。|
|[/OUT](out-output-file-name.md)|指定输出文件名。|
|[/PDB](pdb-use-program-database.md)|创建 PDB 文件。|
|[/PDBALTPATH](pdbaltpath-use-alternate-pdb-path.md)|使用备用位置来保存 PDB 文件。|
|[/PDBSTRIPPED](pdbstripped-strip-private-symbols.md)|创建没有私有符号的 PDB 文件。|
|[/PGD](pgd-specify-database-for-profile-guided-optimizations.md)|*`.pgd`* 为按配置文件优化指定一个文件。|
|[/POGOSAFEMODE](pogosafemode-linker-option.md)|**过时** 创建线程安全的 PGO 检测生成。|
|[/PROFILE](profile-performance-tools-profiler.md)|生成一个可与“性能工具”探查器结合使用的输出文件。|
|[/RELEASE](release-set-the-checksum.md)|在标头中设置校验和 *`.exe`* 。|
|[/SAFESEH](safeseh-image-has-safe-exception-handlers.md)|指定映像将包含安全异常处理程序表。|
|[/SECTION](section-specify-section-attributes.md)|重写节的特性。|
|[/SOURCELINK](sourcelink.md)|指定要添加到 PDB 的 SourceLink 文件。|
|[/STACK](stack-stack-allocations.md)|设置堆栈的大小（以字节为单位）。|
|[/STUB](stub-ms-dos-stub-file-name.md)|将 MS-DOS 存根程序附加到 Win32 程序。|
|[/SUBSYSTEM](subsystem-specify-subsystem.md)|告知操作系统如何运行 *`.exe`* 文件。|
|[/SWAPRUN](swaprun-load-linker-output-to-swap-file.md)|通知操作系统在运行链接器输出之前将其复制到交换文件中。|
|[/TLBID](tlbid-specify-resource-id-for-typelib.md)|指定链接器生成的类型库的资源 ID。|
|[/TLBOUT](tlbout-name-dot-tlb-file.md)|指定 *`.tlb`* 文件和其他 MIDL 输出文件的名称。|
|[/TSAWARE](tsaware-create-terminal-server-aware-application.md)|创建专为在终端服务器下运行而设计的应用程序。|
|[/USEPROFILE](useprofile.md)|使用按配置文件优化定型数据创建优化的映像。|
|[/VERBOSE](verbose-print-progress-messages.md)|打印链接器进度消息。|
|[/VERSION](version-version-information.md)|分配版本号。|
|[/WHOLEARCHIVE](wholearchive-include-all-library-object-files.md)|包括指定静态库中的每个对象文件。|
|[/WINMD](winmd-generate-windows-metadata.md)|允许生成 Windows 运行时元数据文件。|
|[/WINMDFILE](winmdfile-specify-winmd-file.md)|指定由 [/WINMD](winmd-generate-windows-metadata.md) 链接器选项生成的 Windows 运行时元数据 (winmd) 输出文件的文件名。|
|[/WINMDKEYFILE](winmdkeyfile-specify-winmd-key-file.md)|指定用来对 Windows 运行时元数据文件进行签名的密钥或密钥对。|
|[/WINMDKEYCONTAINER](winmdkeycontainer-specify-key-container.md)|指定用来对 Windows 元数据文件进行签名的密钥容器。|
|[/WINMDDELAYSIGN](winmddelaysign-partially-sign-a-winmd.md)|通过将公钥放置在 winmd 文件中，对 Windows 运行时元数据 (winmd) 文件进行部分签名。|
|[/WX](wx-treat-linker-warnings-as-errors.md)|将链接器警告视为错误。|

<sup>16.1</sup> 从 Visual Studio 2019 版本16.1 开始，此选项可用。

## <a name="see-also"></a>另请参阅

[C/C++ 生成参考](c-cpp-building-reference.md)\
[MSVC 链接器参考](linking.md)
