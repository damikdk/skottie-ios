Empty ios app xcode project made with files from skottie ios example

## How to build your skia
1. Clone [skia](https://github.com/google/skia). Check https://skia.org/docs/user/download/ and https://skia.org/docs/user/build/ for help
2. Go to skia folder, prepare it
```
tools/git-sync-deps
```
3. Create output folder for skia build and config. You will change this `args.gn` file later for changes
```
mkdir -p out/ios_arm64_mtl cat > out/ios_arm64_mtl/args.gn <<EOM                                                                                                                
target_os="ios"
target_cpu="arm64"
skia_use_piex=true
skia_use_sfntly=false
skia_use_system_expat=false
skia_use_system_libjpeg_turbo=false
skia_use_system_libpng=false
skia_use_system_libwebp=false
skia_use_system_zlib=false
skia_enable_tools=false
is_official_build=true
skia_enable_skottie=true
is_debug=false
skia_enable_pdf=false
skia_enable_flutter_defines=true
paragraph_tests_enabled=false
is_component_build=false
skia_use_harfbuzz=false
skia_use_icu=false
skia_enable_skunicode=true
skia_use_metal=true
skia_use_gl=false
cc="clang"
cxx="clang++"
ios_min_target="14.0"
extra_cflags=["-target", "arm64-apple-ios", "-fembed-bitcode"]
extra_asmflags=["-target", "arm64-apple-ios", "-fembed-bitcode"]
extra_ldflags=["-target", "arm64-apple-ios", "-fembed-bitcode"]
EOM
```
4. Build skia with
```
bin/gn gen out/ios_arm64_mtl
ninja -C out/ios_arm64_mtl
```
5. Copy result `.a` files to `skia-libs/libs` folder in this project
6. (Optional) If you want to update headers, grab it from skia with this script and move it from `git/Static/starified-skia-libs/headers` to `skia-libs/headers` folder in this project
```
find {client_utils,include,tools,modules/*/include,modules/skottie/src/text,src/core,modules/skcms}/ -iname "*.h" | while read line; do dst=`echo $line | sed -E 's|(.*)$|git/Static/starified-skia-libs/headers/\1|'`; [ -z "$dst" ] && continue; mkdir -p `dirname $dst`; cp $line $dst; done
```
7. Run this project on mac or real device (not simulator)
