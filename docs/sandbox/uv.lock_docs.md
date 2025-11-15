# Documentation: sandbox/uv.lock

## File Metadata

- **Path**: `sandbox/uv.lock`
- **Size**: 85251 bytes
- **Type**: .lock
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sandbox/uv.lock`.

## Original Source Code

```lock
version = 1
revision = 2
requires-python = ">=3.10"

[[package]]
name = "annotated-types"
version = "0.7.0"
source = { registry = "https://pypi.tuna.tsinghua.edu.cn/simple" }
sdist = { url = "https://pypi.tuna.tsinghua.edu.cn/packages/ee/67/531ea369ba64dcff5ec9c3402f9f51bf748cec26dde048a2f973a4eea7f5/annotated_types-0.7.0.tar.gz", hash = "sha256:aff07c09a53a08bc8cfccb9c85b05f1aa9a2a6f23728d790723543408344ce89", size = 16081, upload-time = "2024-05-20T21:33:25.928Z" }
wheels = [
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/78/b6/6307fbef88d9b5ee7421e68d78a9f162e0da4900bc5f5793f6d3d0e34fb8/annotated_types-0.7.0-py3-none-any.whl", hash = "sha256:1f02e8b43a8fbbc3f3e0d4f0f4bfc8131bcb4eebe8849b8e5c773f3a1c582a53", size = 13643, upload-time = "2024-05-20T21:33:24.1Z" },
]

[[package]]
name = "anyio"
version = "4.9.0"
source = { registry = "https://pypi.tuna.tsinghua.edu.cn/simple" }
dependencies = [
    { name = "exceptiongroup", marker = "python_full_version < '3.11'" },
    { name = "idna" },
    { name = "sniffio" },
    { name = "typing-extensions", marker = "python_full_version < '3.13'" },
]
sdist = { url = "https://pypi.tuna.tsinghua.edu.cn/packages/95/7d/4c1bd541d4dffa1b52bd83fb8527089e097a106fc90b467a7313b105f840/anyio-4.9.0.tar.gz", hash = "sha256:673c0c244e15788651a4ff38710fea9675823028a6f08a5eda409e0c9840a028", size = 190949, upload-time = "2025-03-17T00:02:54.77Z" }
wheels = [
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/a1/ee/48ca1a7c89ffec8b6a0c5d02b89c305671d5ffd8d3c94acf8b8c408575bb/anyio-4.9.0-py3-none-any.whl", hash = "sha256:9f76d541cad6e36af7beb62e978876f3b41e3e04f2c1fbf0884604c0a9c4d93c", size = 100916, upload-time = "2025-03-17T00:02:52.713Z" },
]

[[package]]
name = "basedpyright"
version = "1.29.1"
source = { registry = "https://pypi.tuna.tsinghua.edu.cn/simple" }
dependencies = [
    { name = "nodejs-wheel-binaries" },
]
sdist = { url = "https://pypi.tuna.tsinghua.edu.cn/packages/b9/18/f5e488eac4960ad9a2e71b95f0d91cf93a982c7f68aa90e4e0554f0bc37e/basedpyright-1.29.1.tar.gz", hash = "sha256:06bbe6c3b50ab4af20f80e154049477a50d8b81d2522eadbc9f472f2f92cd44b", size = 21773469, upload-time = "2025-04-23T13:29:42.47Z" }
wheels = [
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/95/1b/1bb837bbb7e259928f33d3c105dfef4f5349ef08b3ef45576801256e3234/basedpyright-1.29.1-py3-none-any.whl", hash = "sha256:b7eb65b9d4aaeeea29a349ac494252032a75a364942d0ac466d7f07ddeacc786", size = 11397959, upload-time = "2025-04-23T13:29:38.106Z" },
]

[[package]]
name = "certifi"
version = "2025.4.26"
source = { registry = "https://pypi.tuna.tsinghua.edu.cn/simple" }
sdist = { url = "https://pypi.tuna.tsinghua.edu.cn/packages/e8/9e/c05b3920a3b7d20d3d3310465f50348e5b3694f4f88c6daf736eef3024c4/certifi-2025.4.26.tar.gz", hash = "sha256:0a816057ea3cdefcef70270d2c515e4506bbc954f417fa5ade2021213bb8f0c6", size = 160705, upload-time = "2025-04-26T02:12:29.51Z" }
wheels = [
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/4a/7e/3db2bd1b1f9e95f7cddca6d6e75e2f2bd9f51b1246e546d88addca0106bd/certifi-2025.4.26-py3-none-any.whl", hash = "sha256:30350364dfe371162649852c63336a15c70c6510c2ad5015b21c2345311805f3", size = 159618, upload-time = "2025-04-26T02:12:27.662Z" },
]

[[package]]
name = "charset-normalizer"
version = "3.4.2"
source = { registry = "https://pypi.tuna.tsinghua.edu.cn/simple" }
sdist = { url = "https://pypi.tuna.tsinghua.edu.cn/packages/e4/33/89c2ced2b67d1c2a61c19c6751aa8902d46ce3dacb23600a283619f5a12d/charset_normalizer-3.4.2.tar.gz", hash = "sha256:5baececa9ecba31eff645232d59845c07aa030f0c81ee70184a90d35099a0e63", size = 126367, upload-time = "2025-05-02T08:34:42.01Z" }
wheels = [
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/95/28/9901804da60055b406e1a1c5ba7aac1276fb77f1dde635aabfc7fd84b8ab/charset_normalizer-3.4.2-cp310-cp310-macosx_10_9_universal2.whl", hash = "sha256:7c48ed483eb946e6c04ccbe02c6b4d1d48e51944b6db70f697e089c193404941", size = 201818, upload-time = "2025-05-02T08:31:46.725Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/d9/9b/892a8c8af9110935e5adcbb06d9c6fe741b6bb02608c6513983048ba1a18/charset_normalizer-3.4.2-cp310-cp310-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:b2d318c11350e10662026ad0eb71bb51c7812fc8590825304ae0bdd4ac283acd", size = 144649, upload-time = "2025-05-02T08:31:48.889Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/7b/a5/4179abd063ff6414223575e008593861d62abfc22455b5d1a44995b7c101/charset_normalizer-3.4.2-cp310-cp310-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl", hash = "sha256:9cbfacf36cb0ec2897ce0ebc5d08ca44213af24265bd56eca54bee7923c48fd6", size = 155045, upload-time = "2025-05-02T08:31:50.757Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/3b/95/bc08c7dfeddd26b4be8c8287b9bb055716f31077c8b0ea1cd09553794665/charset_normalizer-3.4.2-cp310-cp310-manylinux_2_17_s390x.manylinux2014_s390x.whl", hash = "sha256:18dd2e350387c87dabe711b86f83c9c78af772c748904d372ade190b5c7c9d4d", size = 147356, upload-time = "2025-05-02T08:31:52.634Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/a8/2d/7a5b635aa65284bf3eab7653e8b4151ab420ecbae918d3e359d1947b4d61/charset_normalizer-3.4.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:8075c35cd58273fee266c58c0c9b670947c19df5fb98e7b66710e04ad4e9ff86", size = 149471, upload-time = "2025-05-02T08:31:56.207Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/ae/38/51fc6ac74251fd331a8cfdb7ec57beba8c23fd5493f1050f71c87ef77ed0/charset_normalizer-3.4.2-cp310-cp310-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:5bf4545e3b962767e5c06fe1738f951f77d27967cb2caa64c28be7c4563e162c", size = 151317, upload-time = "2025-05-02T08:31:57.613Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/b7/17/edee1e32215ee6e9e46c3e482645b46575a44a2d72c7dfd49e49f60ce6bf/charset_normalizer-3.4.2-cp310-cp310-musllinux_1_2_aarch64.whl", hash = "sha256:7a6ab32f7210554a96cd9e33abe3ddd86732beeafc7a28e9955cdf22ffadbab0", size = 146368, upload-time = "2025-05-02T08:31:59.468Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/26/2c/ea3e66f2b5f21fd00b2825c94cafb8c326ea6240cd80a91eb09e4a285830/charset_normalizer-3.4.2-cp310-cp310-musllinux_1_2_i686.whl", hash = "sha256:b33de11b92e9f75a2b545d6e9b6f37e398d86c3e9e9653c4864eb7e89c5773ef", size = 154491, upload-time = "2025-05-02T08:32:01.219Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/52/47/7be7fa972422ad062e909fd62460d45c3ef4c141805b7078dbab15904ff7/charset_normalizer-3.4.2-cp310-cp310-musllinux_1_2_ppc64le.whl", hash = "sha256:8755483f3c00d6c9a77f490c17e6ab0c8729e39e6390328e42521ef175380ae6", size = 157695, upload-time = "2025-05-02T08:32:03.045Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/2f/42/9f02c194da282b2b340f28e5fb60762de1151387a36842a92b533685c61e/charset_normalizer-3.4.2-cp310-cp310-musllinux_1_2_s390x.whl", hash = "sha256:68a328e5f55ec37c57f19ebb1fdc56a248db2e3e9ad769919a58672958e8f366", size = 154849, upload-time = "2025-05-02T08:32:04.651Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/67/44/89cacd6628f31fb0b63201a618049be4be2a7435a31b55b5eb1c3674547a/charset_normalizer-3.4.2-cp310-cp310-musllinux_1_2_x86_64.whl", hash = "sha256:21b2899062867b0e1fde9b724f8aecb1af14f2778d69aacd1a5a1853a597a5db", size = 150091, upload-time = "2025-05-02T08:32:06.719Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/1f/79/4b8da9f712bc079c0f16b6d67b099b0b8d808c2292c937f267d816ec5ecc/charset_normalizer-3.4.2-cp310-cp310-win32.whl", hash = "sha256:e8082b26888e2f8b36a042a58307d5b917ef2b1cacab921ad3323ef91901c71a", size = 98445, upload-time = "2025-05-02T08:32:08.66Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/7d/d7/96970afb4fb66497a40761cdf7bd4f6fca0fc7bafde3a84f836c1f57a926/charset_normalizer-3.4.2-cp310-cp310-win_amd64.whl", hash = "sha256:f69a27e45c43520f5487f27627059b64aaf160415589230992cec34c5e18a509", size = 105782, upload-time = "2025-05-02T08:32:10.46Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/05/85/4c40d00dcc6284a1c1ad5de5e0996b06f39d8232f1031cd23c2f5c07ee86/charset_normalizer-3.4.2-cp311-cp311-macosx_10_9_universal2.whl", hash = "sha256:be1e352acbe3c78727a16a455126d9ff83ea2dfdcbc83148d2982305a04714c2", size = 198794, upload-time = "2025-05-02T08:32:11.945Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/41/d9/7a6c0b9db952598e97e93cbdfcb91bacd89b9b88c7c983250a77c008703c/charset_normalizer-3.4.2-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:aa88ca0b1932e93f2d961bf3addbb2db902198dca337d88c89e1559e066e7645", size = 142846, upload-time = "2025-05-02T08:32:13.946Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/66/82/a37989cda2ace7e37f36c1a8ed16c58cf48965a79c2142713244bf945c89/charset_normalizer-3.4.2-cp311-cp311-manylinux_2_17_ppc64le.manylinux2014_ppc64le.whl", hash = "sha256:d524ba3f1581b35c03cb42beebab4a13e6cdad7b36246bd22541fa585a56cccd", size = 153350, upload-time = "2025-05-02T08:32:15.873Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/df/68/a576b31b694d07b53807269d05ec3f6f1093e9545e8607121995ba7a8313/charset_normalizer-3.4.2-cp311-cp311-manylinux_2_17_s390x.manylinux2014_s390x.whl", hash = "sha256:28a1005facc94196e1fb3e82a3d442a9d9110b8434fc1ded7a24a2983c9888d8", size = 145657, upload-time = "2025-05-02T08:32:17.283Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/92/9b/ad67f03d74554bed3aefd56fe836e1623a50780f7c998d00ca128924a499/charset_normalizer-3.4.2-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:fdb20a30fe1175ecabed17cbf7812f7b804b8a315a25f24678bcdf120a90077f", size = 147260, upload-time = "2025-05-02T08:32:18.807Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/a6/e6/8aebae25e328160b20e31a7e9929b1578bbdc7f42e66f46595a432f8539e/charset_normalizer-3.4.2-cp311-cp311-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.ma

... [Content truncated - file is 85251 bytes] ...

b989c0b85ed3a6a41614b104204a788c20e/wrapt-1.17.2-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:5bb1d0dbf99411f3d871deb6faa9aabb9d4e744d67dcaaa05399af89d847a91d", size = 88721, upload-time = "2025-01-14T10:34:07.163Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/25/cb/7262bc1b0300b4b64af50c2720ef958c2c1917525238d661c3e9a2b71b7b/wrapt-1.17.2-cp312-cp312-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:d18a4865f46b8579d44e4fe1e2bcbc6472ad83d98e22a26c963d46e4c125ef0b", size = 80899, upload-time = "2025-01-14T10:34:09.82Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/2a/5a/04cde32b07a7431d4ed0553a76fdb7a61270e78c5fd5a603e190ac389f14/wrapt-1.17.2-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:bc570b5f14a79734437cb7b0500376b6b791153314986074486e0b0fa8d71d98", size = 89222, upload-time = "2025-01-14T10:34:11.258Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/09/28/2e45a4f4771fcfb109e244d5dbe54259e970362a311b67a965555ba65026/wrapt-1.17.2-cp312-cp312-musllinux_1_2_aarch64.whl", hash = "sha256:6d9187b01bebc3875bac9b087948a2bccefe464a7d8f627cf6e48b1bbae30f82", size = 86707, upload-time = "2025-01-14T10:34:12.49Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/c6/d2/dcb56bf5f32fcd4bd9aacc77b50a539abdd5b6536872413fd3f428b21bed/wrapt-1.17.2-cp312-cp312-musllinux_1_2_i686.whl", hash = "sha256:9e8659775f1adf02eb1e6f109751268e493c73716ca5761f8acb695e52a756ae", size = 79685, upload-time = "2025-01-14T10:34:15.043Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/80/4e/eb8b353e36711347893f502ce91c770b0b0929f8f0bed2670a6856e667a9/wrapt-1.17.2-cp312-cp312-musllinux_1_2_x86_64.whl", hash = "sha256:e8b2816ebef96d83657b56306152a93909a83f23994f4b30ad4573b00bd11bb9", size = 87567, upload-time = "2025-01-14T10:34:16.563Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/17/27/4fe749a54e7fae6e7146f1c7d914d28ef599dacd4416566c055564080fe2/wrapt-1.17.2-cp312-cp312-win32.whl", hash = "sha256:468090021f391fe0056ad3e807e3d9034e0fd01adcd3bdfba977b6fdf4213ea9", size = 36672, upload-time = "2025-01-14T10:34:17.727Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/15/06/1dbf478ea45c03e78a6a8c4be4fdc3c3bddea5c8de8a93bc971415e47f0f/wrapt-1.17.2-cp312-cp312-win_amd64.whl", hash = "sha256:ec89ed91f2fa8e3f52ae53cd3cf640d6feff92ba90d62236a81e4e563ac0e991", size = 38865, upload-time = "2025-01-14T10:34:19.577Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/ce/b9/0ffd557a92f3b11d4c5d5e0c5e4ad057bd9eb8586615cdaf901409920b14/wrapt-1.17.2-cp313-cp313-macosx_10_13_universal2.whl", hash = "sha256:6ed6ffac43aecfe6d86ec5b74b06a5be33d5bb9243d055141e8cabb12aa08125", size = 53800, upload-time = "2025-01-14T10:34:21.571Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/c0/ef/8be90a0b7e73c32e550c73cfb2fa09db62234227ece47b0e80a05073b375/wrapt-1.17.2-cp313-cp313-macosx_10_13_x86_64.whl", hash = "sha256:35621ae4c00e056adb0009f8e86e28eb4a41a4bfa8f9bfa9fca7d343fe94f998", size = 38824, upload-time = "2025-01-14T10:34:22.999Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/36/89/0aae34c10fe524cce30fe5fc433210376bce94cf74d05b0d68344c8ba46e/wrapt-1.17.2-cp313-cp313-macosx_11_0_arm64.whl", hash = "sha256:a604bf7a053f8362d27eb9fefd2097f82600b856d5abe996d623babd067b1ab5", size = 38920, upload-time = "2025-01-14T10:34:25.386Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/3b/24/11c4510de906d77e0cfb5197f1b1445d4fec42c9a39ea853d482698ac681/wrapt-1.17.2-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:5cbabee4f083b6b4cd282f5b817a867cf0b1028c54d445b7ec7cfe6505057cf8", size = 88690, upload-time = "2025-01-14T10:34:28.058Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/71/d7/cfcf842291267bf455b3e266c0c29dcb675b5540ee8b50ba1699abf3af45/wrapt-1.17.2-cp313-cp313-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:49703ce2ddc220df165bd2962f8e03b84c89fee2d65e1c24a7defff6f988f4d6", size = 80861, upload-time = "2025-01-14T10:34:29.167Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/d5/66/5d973e9f3e7370fd686fb47a9af3319418ed925c27d72ce16b791231576d/wrapt-1.17.2-cp313-cp313-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:8112e52c5822fc4253f3901b676c55ddf288614dc7011634e2719718eaa187dc", size = 89174, upload-time = "2025-01-14T10:34:31.702Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/a7/d3/8e17bb70f6ae25dabc1aaf990f86824e4fd98ee9cadf197054e068500d27/wrapt-1.17.2-cp313-cp313-musllinux_1_2_aarch64.whl", hash = "sha256:9fee687dce376205d9a494e9c121e27183b2a3df18037f89d69bd7b35bcf59e2", size = 86721, upload-time = "2025-01-14T10:34:32.91Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/6f/54/f170dfb278fe1c30d0ff864513cff526d624ab8de3254b20abb9cffedc24/wrapt-1.17.2-cp313-cp313-musllinux_1_2_i686.whl", hash = "sha256:18983c537e04d11cf027fbb60a1e8dfd5190e2b60cc27bc0808e653e7b218d1b", size = 79763, upload-time = "2025-01-14T10:34:34.903Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/4a/98/de07243751f1c4a9b15c76019250210dd3486ce098c3d80d5f729cba029c/wrapt-1.17.2-cp313-cp313-musllinux_1_2_x86_64.whl", hash = "sha256:703919b1633412ab54bcf920ab388735832fdcb9f9a00ae49387f0fe67dad504", size = 87585, upload-time = "2025-01-14T10:34:36.13Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/f9/f0/13925f4bd6548013038cdeb11ee2cbd4e37c30f8bfd5db9e5a2a370d6e20/wrapt-1.17.2-cp313-cp313-win32.whl", hash = "sha256:abbb9e76177c35d4e8568e58650aa6926040d6a9f6f03435b7a522bf1c487f9a", size = 36676, upload-time = "2025-01-14T10:34:37.962Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/bf/ae/743f16ef8c2e3628df3ddfd652b7d4c555d12c84b53f3d8218498f4ade9b/wrapt-1.17.2-cp313-cp313-win_amd64.whl", hash = "sha256:69606d7bb691b50a4240ce6b22ebb319c1cfb164e5f6569835058196e0f3a845", size = 38871, upload-time = "2025-01-14T10:34:39.13Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/3d/bc/30f903f891a82d402ffb5fda27ec1d621cc97cb74c16fea0b6141f1d4e87/wrapt-1.17.2-cp313-cp313t-macosx_10_13_universal2.whl", hash = "sha256:4a721d3c943dae44f8e243b380cb645a709ba5bd35d3ad27bc2ed947e9c68192", size = 56312, upload-time = "2025-01-14T10:34:40.604Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/8a/04/c97273eb491b5f1c918857cd26f314b74fc9b29224521f5b83f872253725/wrapt-1.17.2-cp313-cp313t-macosx_10_13_x86_64.whl", hash = "sha256:766d8bbefcb9e00c3ac3b000d9acc51f1b399513f44d77dfe0eb026ad7c9a19b", size = 40062, upload-time = "2025-01-14T10:34:45.011Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/4e/ca/3b7afa1eae3a9e7fefe499db9b96813f41828b9fdb016ee836c4c379dadb/wrapt-1.17.2-cp313-cp313t-macosx_11_0_arm64.whl", hash = "sha256:e496a8ce2c256da1eb98bd15803a79bee00fc351f5dfb9ea82594a3f058309e0", size = 40155, upload-time = "2025-01-14T10:34:47.25Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/89/be/7c1baed43290775cb9030c774bc53c860db140397047cc49aedaf0a15477/wrapt-1.17.2-cp313-cp313t-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:40d615e4fe22f4ad3528448c193b218e077656ca9ccb22ce2cb20db730f8d306", size = 113471, upload-time = "2025-01-14T10:34:50.934Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/32/98/4ed894cf012b6d6aae5f5cc974006bdeb92f0241775addad3f8cd6ab71c8/wrapt-1.17.2-cp313-cp313t-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:a5aaeff38654462bc4b09023918b7f21790efb807f54c000a39d41d69cf552cb", size = 101208, upload-time = "2025-01-14T10:34:52.297Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/ea/fd/0c30f2301ca94e655e5e057012e83284ce8c545df7661a78d8bfca2fac7a/wrapt-1.17.2-cp313-cp313t-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:9a7d15bbd2bc99e92e39f49a04653062ee6085c0e18b3b7512a4f2fe91f2d681", size = 109339, upload-time = "2025-01-14T10:34:53.489Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/75/56/05d000de894c4cfcb84bcd6b1df6214297b8089a7bd324c21a4765e49b14/wrapt-1.17.2-cp313-cp313t-musllinux_1_2_aarch64.whl", hash = "sha256:e3890b508a23299083e065f435a492b5435eba6e304a7114d2f919d400888cc6", size = 110232, upload-time = "2025-01-14T10:34:55.327Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/53/f8/c3f6b2cf9b9277fb0813418e1503e68414cd036b3b099c823379c9575e6d/wrapt-1.17.2-cp313-cp313t-musllinux_1_2_i686.whl", hash = "sha256:8c8b293cd65ad716d13d8dd3624e42e5a19cc2a2f1acc74b30c2c13f15cb61a6", size = 100476, upload-time = "2025-01-14T10:34:58.055Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/a7/b1/0bb11e29aa5139d90b770ebbfa167267b1fc548d2302c30c8f7572851738/wrapt-1.17.2-cp313-cp313t-musllinux_1_2_x86_64.whl", hash = "sha256:4c82b8785d98cdd9fed4cac84d765d234ed3251bd6afe34cb7ac523cb93e8b4f", size = 106377, upload-time = "2025-01-14T10:34:59.3Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/6a/e1/0122853035b40b3f333bbb25f1939fc1045e21dd518f7f0922b60c156f7c/wrapt-1.17.2-cp313-cp313t-win32.whl", hash = "sha256:13e6afb7fe71fe7485a4550a8844cc9ffbe263c0f1a1eea569bc7091d4898555", size = 37986, upload-time = "2025-01-14T10:35:00.498Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/09/5e/1655cf481e079c1f22d0cabdd4e51733679932718dc23bf2db175f329b76/wrapt-1.17.2-cp313-cp313t-win_amd64.whl", hash = "sha256:eaf675418ed6b3b31c7a989fd007fa7c3be66ce14e5c3b27336383604c9da85c", size = 40750, upload-time = "2025-01-14T10:35:03.378Z" },
    { url = "https://pypi.tuna.tsinghua.edu.cn/packages/2d/82/f56956041adef78f849db6b289b282e72b55ab8045a75abad81898c28d19/wrapt-1.17.2-py3-none-any.whl", hash = "sha256:b18f2d1533a71f069c7f82d524a52599053d4c7166e9dd374ae2136b7f40f7c8", size = 23594, upload-time = "2025-01-14T10:35:44.018Z" },
]

```

## Detailed Analysis

### File Role in Repository

The file `sandbox/uv.lock` is located in the `sandbox` directory.

### Architecture Context

Files in this location typically handle concerns related to sandbox.

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

- [Makefile](Makefile_docs.md)
- [README.md](README.md_docs.md)
- [docker-compose.yml](docker-compose.yml_docs.md)
- [pyproject.toml](pyproject.toml_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
