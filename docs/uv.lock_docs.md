# Documentation: uv.lock

## File Metadata

- **Path**: `uv.lock`
- **Size**: 1028403 bytes
- **Type**: .lock
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `uv.lock`.

## Original Source Code

```lock
version = 1
revision = 3
requires-python = ">=3.10, <3.13"
resolution-markers = [
    "python_full_version >= '3.12' and sys_platform == 'darwin'",
    "python_full_version >= '3.12' and platform_machine == 'aarch64' and sys_platform == 'linux'",
    "(python_full_version >= '3.12' and platform_machine != 'aarch64' and sys_platform == 'linux') or (python_full_version >= '3.12' and sys_platform != 'darwin' and sys_platform != 'linux')",
    "python_full_version == '3.11.*' and sys_platform == 'darwin'",
    "python_full_version == '3.11.*' and platform_machine == 'aarch64' and sys_platform == 'linux'",
    "(python_full_version == '3.11.*' and platform_machine != 'aarch64' and sys_platform == 'linux') or (python_full_version == '3.11.*' and sys_platform != 'darwin' and sys_platform != 'linux')",
    "python_full_version < '3.11' and sys_platform == 'darwin'",
    "python_full_version < '3.11' and platform_machine == 'aarch64' and sys_platform == 'linux'",
    "(python_full_version < '3.11' and platform_machine != 'aarch64' and sys_platform == 'linux') or (python_full_version < '3.11' and sys_platform != 'darwin' and sys_platform != 'linux')",
]

[[package]]
name = "aiofiles"
version = "24.1.0"
source = { registry = "https://pypi.tuna.tsinghua.edu.cn/simple" }
sdist = { url = "https://pypi.tuna.tsinghua.edu.cn/packages/0b/03/a88171e277e8caa88a4c77808c20ebb04ba74cc4681bf1e9416c862de237/aiofiles-24.1.0.tar.gz", hash = "sha256:22a075c9e5a3810f0c2e48f3008c94d68c65d763b9b03857924c99e57355166c", size = 30247, upload-time = "2024-06-24T11:02:03.584Z" }
wheels = [
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/a5/45/30bb92d442636f570cb5651bc661f52b610e2eec3f891a5dc3a4c3667db0/aiofiles-24.1.0-py3-none-any.whl", hash = "sha256:b4ec55f4195e3eb5d7abd1bf7e061763e864dd4954231fb8539a0ef8bb8260e5", size = 15896, upload-time = "2024-06-24T11:02:01.529Z" },
]

[[package]]
name = "aiohappyeyeballs"
version = "2.6.1"
source = { registry = "https://pypi.tuna.tsinghua.edu.cn/simple" }
sdist = { url = "https://pypi.tuna.tsinghua.edu.cn/packages/26/30/f84a107a9c4331c14b2b586036f40965c128aa4fee4dda5d3d51cb14ad54/aiohappyeyeballs-2.6.1.tar.gz", hash = "sha256:c3f9d0113123803ccadfdf3f0faa505bc78e6a72d1cc4806cbd719826e943558", size = 22760, upload-time = "2025-03-12T01:42:48.764Z" }
wheels = [
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/0f/15/5bf3b99495fb160b63f95972b81750f18f7f4e02ad051373b669d17d44f2/aiohappyeyeballs-2.6.1-py3-none-any.whl", hash = "sha256:f349ba8f4b75cb25c99c5c2d84e997e485204d2902a9597802b0371f09331fb8", size = 15265, upload-time = "2025-03-12T01:42:47.083Z" },
]

[[package]]
name = "aiohttp"
version = "3.12.15"
source = { registry = "https://pypi.tuna.tsinghua.edu.cn/simple" }
dependencies = [
    { name = "aiohappyeyeballs" },
    { name = "aiosignal" },
    { name = "async-timeout", marker = "python_full_version < '3.11'" },
    { name = "attrs" },
    { name = "frozenlist" },
    { name = "multidict" },
    { name = "propcache" },
    { name = "yarl" },
]
sdist = { url = "https://pypi.tuna.tsinghua.edu.cn/packages/9b/e7/d92a237d8802ca88483906c388f7c201bbe96cd80a165ffd0ac2f6a8d59f/aiohttp-3.12.15.tar.gz", hash = "sha256:4fc61385e9c98d72fcdf47e6dd81833f47b2f77c114c29cd64a361be57a763a2", size = 7823716, upload-time = "2025-07-29T05:52:32.215Z" }
wheels = [
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/47/dc/ef9394bde9080128ad401ac7ede185267ed637df03b51f05d14d1c99ad67/aiohttp-3.12.15-cp310-cp310-macosx_10_9_universal2.whl", hash = "sha256:b6fc902bff74d9b1879ad55f5404153e2b33a82e72a95c89cec5eb6cc9e92fbc", size = 703921, upload-time = "2025-07-29T05:49:43.584Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/8f/42/63fccfc3a7ed97eb6e1a71722396f409c46b60a0552d8a56d7aad74e0df5/aiohttp-3.12.15-cp310-cp310-macosx_10_9_x86_64.whl", hash = "sha256:098e92835b8119b54c693f2f88a1dec690e20798ca5f5fe5f0520245253ee0af", size = 480288, upload-time = "2025-07-29T05:49:47.851Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/9c/a2/7b8a020549f66ea2a68129db6960a762d2393248f1994499f8ba9728bbed/aiohttp-3.12.15-cp310-cp310-macosx_11_0_arm64.whl", hash = "sha256:40b3fee496a47c3b4a39a731954c06f0bd9bd3e8258c059a4beb76ac23f8e421", size = 468063, upload-time = "2025-07-29T05:49:49.789Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/8f/f5/d11e088da9176e2ad8220338ae0000ed5429a15f3c9dfd983f39105399cd/aiohttp-3.12.15-cp310-cp310-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:2ce13fcfb0bb2f259fb42106cdc63fa5515fb85b7e87177267d89a771a660b79", size = 1650122, upload-time = "2025-07-29T05:49:51.874Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/b0/6b/b60ce2757e2faed3d70ed45dafee48cee7bfb878785a9423f7e883f0639c/aiohttp-3.12.15-cp310-cp310-manylinux_2_17_armv7l.manylinux2014_armv7l.manylinux_2_31_armv7l.whl", hash = "sha256:3beb14f053222b391bf9cf92ae82e0171067cc9c8f52453a0f1ec7c37df12a77", size = 1624176, upload-time = "2025-07-29T05:49:53.805Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/dd/de/8c9fde2072a1b72c4fadecf4f7d4be7a85b1d9a4ab333d8245694057b4c6/aiohttp-3.12.15-cp310-cp310-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl", hash = "sha256:4c39e87afe48aa3e814cac5f535bc6199180a53e38d3f51c5e2530f5aa4ec58c", size = 1696583, upload-time = "2025-07-29T05:49:55.338Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/0c/ad/07f863ca3d895a1ad958a54006c6dafb4f9310f8c2fdb5f961b8529029d3/aiohttp-3.12.15-cp310-cp310-manylinux_2_17_s390x.manylinux2014_s390x.whl", hash = "sha256:d5f1b4ce5bc528a6ee38dbf5f39bbf11dd127048726323b72b8e85769319ffc4", size = 1738896, upload-time = "2025-07-29T05:49:57.045Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/20/43/2bd482ebe2b126533e8755a49b128ec4e58f1a3af56879a3abdb7b42c54f/aiohttp-3.12.15-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:1004e67962efabbaf3f03b11b4c43b834081c9e3f9b32b16a7d97d4708a9abe6", size = 1643561, upload-time = "2025-07-29T05:49:58.762Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/23/40/2fa9f514c4cf4cbae8d7911927f81a1901838baf5e09a8b2c299de1acfe5/aiohttp-3.12.15-cp310-cp310-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:8faa08fcc2e411f7ab91d1541d9d597d3a90e9004180edb2072238c085eac8c2", size = 1583685, upload-time = "2025-07-29T05:50:00.375Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/b8/c3/94dc7357bc421f4fb978ca72a201a6c604ee90148f1181790c129396ceeb/aiohttp-3.12.15-cp310-cp310-musllinux_1_2_aarch64.whl", hash = "sha256:fe086edf38b2222328cdf89af0dde2439ee173b8ad7cb659b4e4c6f385b2be3d", size = 1627533, upload-time = "2025-07-29T05:50:02.306Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/bf/3f/1f8911fe1844a07001e26593b5c255a685318943864b27b4e0267e840f95/aiohttp-3.12.15-cp310-cp310-musllinux_1_2_armv7l.whl", hash = "sha256:79b26fe467219add81d5e47b4a4ba0f2394e8b7c7c3198ed36609f9ba161aecb", size = 1638319, upload-time = "2025-07-29T05:50:04.282Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/4e/46/27bf57a99168c4e145ffee6b63d0458b9c66e58bb70687c23ad3d2f0bd17/aiohttp-3.12.15-cp310-cp310-musllinux_1_2_i686.whl", hash = "sha256:b761bac1192ef24e16706d761aefcb581438b34b13a2f069a6d343ec8fb693a5", size = 1613776, upload-time = "2025-07-29T05:50:05.863Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/0f/7e/1d2d9061a574584bb4ad3dbdba0da90a27fdc795bc227def3a46186a8bc1/aiohttp-3.12.15-cp310-cp310-musllinux_1_2_ppc64le.whl", hash = "sha256:e153e8adacfe2af562861b72f8bc47f8a5c08e010ac94eebbe33dc21d677cd5b", size = 1693359, upload-time = "2025-07-29T05:50:07.563Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/08/98/bee429b52233c4a391980a5b3b196b060872a13eadd41c3a34be9b1469ed/aiohttp-3.12.15-cp310-cp310-musllinux_1_2_s390x.whl", hash = "sha256:fc49c4de44977aa8601a00edbf157e9a421f227aa7eb477d9e3df48343311065", size = 1716598, upload-time = "2025-07-29T05:50:09.33Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/57/39/b0314c1ea774df3392751b686104a3938c63ece2b7ce0ba1ed7c0b4a934f/aiohttp-3.12.15-cp310-cp310-musllinux_1_2_x86_64.whl", hash = "sha256:2776c7ec89c54a47029940177e75c8c07c29c66f73464784971d6a81904ce9d1", size = 1644940, upload-time = "2025-07-29T05:50:11.334Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/1b/83/3dacb8d3f8f512c8ca43e3fa8a68b20583bd25636ffa4e56ee841ffd79ae/aiohttp-3.12.15-cp310-cp310-win32.whl", hash = "sha256:2c7d81a277fa78b2203ab626ced1487420e8c11a8e373707ab72d189fcdad20a", size = 429239, upload-time = "2025-07-29T05:50:12.803Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/eb/f9/470b5daba04d558c9673ca2034f28d067f3202a40e17804425f0c331c89f/aiohttp-3.12.15-cp310-cp310-win_amd64.whl", hash = "sha256:83603f881e11f0f710f8e2327817c82e79431ec976448839f3cd05d7afe8f830", size = 452297, upload-time = "2025-07-29T05:50:14.266Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/20/19/9e86722ec8e835959bd97ce8c1efa78cf361fa4531fca372551abcc9cdd6/aiohttp-3.12.15-cp311-cp311-macosx_10_9_universal2.whl", hash = "sha256:d3ce17ce0220383a0f9ea07175eeaa6aa13ae5a41f30bc61d84df17f0e9b1117", size = 711246, upload-time = "2025-07-29T05:50:15.937Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/71/f9/0a31fcb1a7d4629ac9d8f01f1cb9242e2f9943f47f5d03215af91c3c1a26/aiohttp-3.12.15-cp311-cp311-macosx_10_9_x86_64.whl", hash = "sha256:010cc9bbd06db80fe234d9003f67e97a10fe003bfbedb40da7d71c1008eda0fe", size = 483515, upload-time = "2025-07-29T05:50:17.442Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/62/6c/94846f576f1d11df0c2e41d3001000527c0fdf63fce7e69b3927a731325d/aiohttp-3.12.15-cp311-cp311-macosx_11_0_arm64.whl", hash = "sha256:3f9d7c55b41ed687b9d7165b17672340187f87a773c98236c987f08c858145a9", size = 471776, upload-time = "2025-07-29T05:50:19.568Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/f8/6c/f766d0aaafcee0447fad0328da780d344489c042e25cd58fde566bf40aed/aio

... [Content truncated - file is 1028403 bytes] ...

.whl", hash = "sha256:77ea385f7dd5b5676d7fd943292ffa18fbf5c72ba98f7d09fc1fb9e819b34c23", size = 633681, upload-time = "2024-07-15T00:14:13.99Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/63/b6/677e65c095d8e12b66b8f862b069bcf1f1d781b9c9c6f12eb55000d57583/zstandard-0.23.0-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:983b6efd649723474f29ed42e1467f90a35a74793437d0bc64a5bf482bedfa0a", size = 4944328, upload-time = "2024-07-15T00:14:16.588Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/59/cc/e76acb4c42afa05a9d20827116d1f9287e9c32b7ad58cc3af0721ce2b481/zstandard-0.23.0-cp311-cp311-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl", hash = "sha256:80a539906390591dd39ebb8d773771dc4db82ace6372c4d41e2d293f8e32b8db", size = 5311955, upload-time = "2024-07-15T00:14:19.389Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/78/e4/644b8075f18fc7f632130c32e8f36f6dc1b93065bf2dd87f03223b187f26/zstandard-0.23.0-cp311-cp311-manylinux_2_17_s390x.manylinux2014_s390x.whl", hash = "sha256:445e4cb5048b04e90ce96a79b4b63140e3f4ab5f662321975679b5f6360b90e2", size = 5344944, upload-time = "2024-07-15T00:14:22.173Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/76/3f/dbafccf19cfeca25bbabf6f2dd81796b7218f768ec400f043edc767015a6/zstandard-0.23.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:fd30d9c67d13d891f2360b2a120186729c111238ac63b43dbd37a5a40670b8ca", size = 5442927, upload-time = "2024-07-15T00:14:24.825Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/0c/c3/d24a01a19b6733b9f218e94d1a87c477d523237e07f94899e1c10f6fd06c/zstandard-0.23.0-cp311-cp311-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:d20fd853fbb5807c8e84c136c278827b6167ded66c72ec6f9a14b863d809211c", size = 4864910, upload-time = "2024-07-15T00:14:26.982Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/1c/a9/cf8f78ead4597264f7618d0875be01f9bc23c9d1d11afb6d225b867cb423/zstandard-0.23.0-cp311-cp311-musllinux_1_1_aarch64.whl", hash = "sha256:ed1708dbf4d2e3a1c5c69110ba2b4eb6678262028afd6c6fbcc5a8dac9cda68e", size = 4935544, upload-time = "2024-07-15T00:14:29.582Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/2c/96/8af1e3731b67965fb995a940c04a2c20997a7b3b14826b9d1301cf160879/zstandard-0.23.0-cp311-cp311-musllinux_1_1_x86_64.whl", hash = "sha256:be9b5b8659dff1f913039c2feee1aca499cfbc19e98fa12bc85e037c17ec6ca5", size = 5467094, upload-time = "2024-07-15T00:14:40.126Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/ff/57/43ea9df642c636cb79f88a13ab07d92d88d3bfe3e550b55a25a07a26d878/zstandard-0.23.0-cp311-cp311-musllinux_1_2_aarch64.whl", hash = "sha256:65308f4b4890aa12d9b6ad9f2844b7ee42c7f7a4fd3390425b242ffc57498f48", size = 4860440, upload-time = "2024-07-15T00:14:42.786Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/46/37/edb78f33c7f44f806525f27baa300341918fd4c4af9472fbc2c3094be2e8/zstandard-0.23.0-cp311-cp311-musllinux_1_2_i686.whl", hash = "sha256:98da17ce9cbf3bfe4617e836d561e433f871129e3a7ac16d6ef4c680f13a839c", size = 4700091, upload-time = "2024-07-15T00:14:45.184Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/c1/f1/454ac3962671a754f3cb49242472df5c2cced4eb959ae203a377b45b1a3c/zstandard-0.23.0-cp311-cp311-musllinux_1_2_ppc64le.whl", hash = "sha256:8ed7d27cb56b3e058d3cf684d7200703bcae623e1dcc06ed1e18ecda39fee003", size = 5208682, upload-time = "2024-07-15T00:14:47.407Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/85/b2/1734b0fff1634390b1b887202d557d2dd542de84a4c155c258cf75da4773/zstandard-0.23.0-cp311-cp311-musllinux_1_2_s390x.whl", hash = "sha256:b69bb4f51daf461b15e7b3db033160937d3ff88303a7bc808c67bbc1eaf98c78", size = 5669707, upload-time = "2024-07-15T00:15:03.529Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/52/5a/87d6971f0997c4b9b09c495bf92189fb63de86a83cadc4977dc19735f652/zstandard-0.23.0-cp311-cp311-musllinux_1_2_x86_64.whl", hash = "sha256:034b88913ecc1b097f528e42b539453fa82c3557e414b3de9d5632c80439a473", size = 5201792, upload-time = "2024-07-15T00:15:28.372Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/79/02/6f6a42cc84459d399bd1a4e1adfc78d4dfe45e56d05b072008d10040e13b/zstandard-0.23.0-cp311-cp311-win32.whl", hash = "sha256:f2d4380bf5f62daabd7b751ea2339c1a21d1c9463f1feb7fc2bdcea2c29c3160", size = 430586, upload-time = "2024-07-15T00:15:32.26Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/be/a2/4272175d47c623ff78196f3c10e9dc7045c1b9caf3735bf041e65271eca4/zstandard-0.23.0-cp311-cp311-win_amd64.whl", hash = "sha256:62136da96a973bd2557f06ddd4e8e807f9e13cbb0bfb9cc06cfe6d98ea90dfe0", size = 495420, upload-time = "2024-07-15T00:15:34.004Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/7b/83/f23338c963bd9de687d47bf32efe9fd30164e722ba27fb59df33e6b1719b/zstandard-0.23.0-cp312-cp312-macosx_10_9_x86_64.whl", hash = "sha256:b4567955a6bc1b20e9c31612e615af6b53733491aeaa19a6b3b37f3b65477094", size = 788713, upload-time = "2024-07-15T00:15:35.815Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/5b/b3/1a028f6750fd9227ee0b937a278a434ab7f7fdc3066c3173f64366fe2466/zstandard-0.23.0-cp312-cp312-macosx_11_0_arm64.whl", hash = "sha256:1e172f57cd78c20f13a3415cc8dfe24bf388614324d25539146594c16d78fcc8", size = 633459, upload-time = "2024-07-15T00:15:37.995Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/26/af/36d89aae0c1f95a0a98e50711bc5d92c144939efc1f81a2fcd3e78d7f4c1/zstandard-0.23.0-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:b0e166f698c5a3e914947388c162be2583e0c638a4703fc6a543e23a88dea3c1", size = 4945707, upload-time = "2024-07-15T00:15:39.872Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/cd/2e/2051f5c772f4dfc0aae3741d5fc72c3dcfe3aaeb461cc231668a4db1ce14/zstandard-0.23.0-cp312-cp312-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl", hash = "sha256:12a289832e520c6bd4dcaad68e944b86da3bad0d339ef7989fb7e88f92e96072", size = 5306545, upload-time = "2024-07-15T00:15:41.75Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/0a/9e/a11c97b087f89cab030fa71206963090d2fecd8eb83e67bb8f3ffb84c024/zstandard-0.23.0-cp312-cp312-manylinux_2_17_s390x.manylinux2014_s390x.whl", hash = "sha256:d50d31bfedd53a928fed6707b15a8dbeef011bb6366297cc435accc888b27c20", size = 5337533, upload-time = "2024-07-15T00:15:44.114Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/fc/79/edeb217c57fe1bf16d890aa91a1c2c96b28c07b46afed54a5dcf310c3f6f/zstandard-0.23.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:72c68dda124a1a138340fb62fa21b9bf4848437d9ca60bd35db36f2d3345f373", size = 5436510, upload-time = "2024-07-15T00:15:46.509Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/81/4f/c21383d97cb7a422ddf1ae824b53ce4b51063d0eeb2afa757eb40804a8ef/zstandard-0.23.0-cp312-cp312-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:53dd9d5e3d29f95acd5de6802e909ada8d8d8cfa37a3ac64836f3bc4bc5512db", size = 4859973, upload-time = "2024-07-15T00:15:49.939Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/ab/15/08d22e87753304405ccac8be2493a495f529edd81d39a0870621462276ef/zstandard-0.23.0-cp312-cp312-musllinux_1_1_aarch64.whl", hash = "sha256:6a41c120c3dbc0d81a8e8adc73312d668cd34acd7725f036992b1b72d22c1772", size = 4936968, upload-time = "2024-07-15T00:15:52.025Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/eb/fa/f3670a597949fe7dcf38119a39f7da49a8a84a6f0b1a2e46b2f71a0ab83f/zstandard-0.23.0-cp312-cp312-musllinux_1_1_x86_64.whl", hash = "sha256:40b33d93c6eddf02d2c19f5773196068d875c41ca25730e8288e9b672897c105", size = 5467179, upload-time = "2024-07-15T00:15:54.971Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/4e/a9/dad2ab22020211e380adc477a1dbf9f109b1f8d94c614944843e20dc2a99/zstandard-0.23.0-cp312-cp312-musllinux_1_2_aarch64.whl", hash = "sha256:9206649ec587e6b02bd124fb7799b86cddec350f6f6c14bc82a2b70183e708ba", size = 4848577, upload-time = "2024-07-15T00:15:57.634Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/08/03/dd28b4484b0770f1e23478413e01bee476ae8227bbc81561f9c329e12564/zstandard-0.23.0-cp312-cp312-musllinux_1_2_i686.whl", hash = "sha256:76e79bc28a65f467e0409098fa2c4376931fd3207fbeb6b956c7c476d53746dd", size = 4693899, upload-time = "2024-07-15T00:16:00.811Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/2b/64/3da7497eb635d025841e958bcd66a86117ae320c3b14b0ae86e9e8627518/zstandard-0.23.0-cp312-cp312-musllinux_1_2_ppc64le.whl", hash = "sha256:66b689c107857eceabf2cf3d3fc699c3c0fe8ccd18df2219d978c0283e4c508a", size = 5199964, upload-time = "2024-07-15T00:16:03.669Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/43/a4/d82decbab158a0e8a6ebb7fc98bc4d903266bce85b6e9aaedea1d288338c/zstandard-0.23.0-cp312-cp312-musllinux_1_2_s390x.whl", hash = "sha256:9c236e635582742fee16603042553d276cca506e824fa2e6489db04039521e90", size = 5655398, upload-time = "2024-07-15T00:16:06.694Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/f2/61/ac78a1263bc83a5cf29e7458b77a568eda5a8f81980691bbc6eb6a0d45cc/zstandard-0.23.0-cp312-cp312-musllinux_1_2_x86_64.whl", hash = "sha256:a8fffdbd9d1408006baaf02f1068d7dd1f016c6bcb7538682622c556e7b68e35", size = 5191313, upload-time = "2024-07-15T00:16:09.758Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/e7/54/967c478314e16af5baf849b6ee9d6ea724ae5b100eb506011f045d3d4e16/zstandard-0.23.0-cp312-cp312-win32.whl", hash = "sha256:dc1d33abb8a0d754ea4763bad944fd965d3d95b5baef6b121c0c9013eaf1907d", size = 430877, upload-time = "2024-07-15T00:16:11.758Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/75/37/872d74bd7739639c4553bf94c84af7d54d8211b626b352bc57f0fd8d1e3f/zstandard-0.23.0-cp312-cp312-win_amd64.whl", hash = "sha256:64585e1dba664dc67c7cdabd56c1e5685233fbb1fc1966cfba2a340ec0dfff7b", size = 495595, upload-time = "2024-07-15T00:16:13.731Z" },
]

```

## Detailed Analysis

### File Role in Repository

The file `uv.lock` is located in the `.` directory.

### Architecture Context

Files in this location typically handle concerns related to .

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [CLAUDE.md](CLAUDE.md_docs.md)
- [Dockerfile](Dockerfile_docs.md)
- [Dockerfile.deps](Dockerfile.deps_docs.md)
- [Dockerfile.scratch.oc9](Dockerfile.scratch.oc9_docs.md)
- [Dockerfile_tei](Dockerfile_tei_docs.md)
- [LICENSE](LICENSE_docs.md)
- [README.md](README.md_docs.md)
- [README_id.md](README_id.md_docs.md)
- [README_ja.md](README_ja.md_docs.md)
- [README_ko.md](README_ko.md_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
