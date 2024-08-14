
show `doc/building-win-x64.md`

modify step:

    1. openssl    
      git clone https://github.com/openssl/openssl.git openssl_1_1_1
      cd openssl_1_1_1
      git checkout OpenSSL_1_1_1-stable
      perl Configure no-shared no-tests debug-VC-WIN64A --prefix="%LibrariesPath%\openssl-1.1.1_cache"
      nmake
      nmake install
      move %LibrariesPath%\openssl-1.1.1_cache\lib\libcrypto.lib %LibrariesPath%\openssl-1.1.1_cache\lib\libcryptoD.lib
      move %LibrariesPath%\openssl-1.1.1_cache\lib\libssl.lib %LibrariesPath%\openssl-1.1.1_cache\lib\libsslD.lib.
      move %LibrariesPath%\openssl-1.1.1_cache\lib\ossl_static %LibrariesPath%\openssl-1.1.1_cache\lib\osslD_static.
      nmake clean
   
      perl Configure no-shared no-tests VC-WIN64A --prefix="%LibrariesPath%\openssl-1.1.1_cache"
      nmake
      nmake install
      nmake clean
      
    2. Qt-5.15.2:
        configure ^ 
        -prefix "%LibrariesPath%\Qt-5.15.2" ^
        -debug-and-release ^
        -force-debug-info ^
        -opensource ^
        -confirm-license ^
        -static ^
        -static-runtime ^
        -opengl es2 -no-angle ^
        -I "%LibrariesPath%\tg_angle\include" ^
        -D "KHRONOS_STATIC=" ^
        -D "DESKTOP_APP_QT_STATIC_ANGLE=" ^
        QMAKE_LIBS_OPENGL_ES2_DEBUG="%LibrariesPath%\tg_angle\out\Debug\tg_angle.lib %LibrariesPath%\zlib\contrib\vstudio\vc14\x64\ZlibStatDebug\zlibstat.lib d3d9.lib dxgi.>
        QMAKE_LIBS_OPENGL_ES2_RELEASE="%LibrariesPath%\tg_angle\out\Release\tg_angle.lib %LibrariesPath%\zlib\contrib\vstudio\vc14\x64\ZlibStatReleaseWithoutAsm\zlibstat.li>
        -egl ^
        QMAKE_LIBS_EGL_DEBUG="%LibrariesPath%\tg_angle\out\Debug\tg_angle.lib %LibrariesPath%\zlib\contrib\vstudio\vc14\x64\ZlibStatDebug\zlibstat.lib d3d9.lib dxgi.lib dxg>
        QMAKE_LIBS_EGL_RELEASE="%LibrariesPath%\tg_angle\out\Release\tg_angle.lib %LibrariesPath%\zlib\contrib\vstudio\vc14\x64\ZlibStatReleaseWithoutAsm\zlibstat.lib d3d9.>
        -openssl-linked ^
        OPENSSL_PREFIX=%LibrariesPath%\openssl_1_1_1_cache ^
        OPENSSL_LIBDIR=%LibrariesPath%\openssl_1_1_1_cache\lib ^
        -I "%LibrariesPath%\mozjpeg" ^
        LIBJPEG_LIBS_DEBUG="%LibrariesPath%\mozjpeg\Debug\jpeg-static.lib" ^
        LIBJPEG_LIBS_RELEASE="%LibrariesPath%\mozjpeg\Release\jpeg-static.lib" ^
        -mp ^
        -nomake examples ^
        -nomake tests ^
        -platform win32-msvc


edit file:

    1. tdesktop\cmake\options_win.cmake
      modify.
          /WX => /WX-
      and add line.
          /w14819
      
    2. tdesktop\Telegram\SourceFiles/media/streaming/media_streaming_video_track.cpp
        line:18
        //extern "C" {
        //extern int __isa_available;
        //}
      
        line:232
        "loop:%5,wait:%6,duration:%7,initialized:%8"
      
        line: 240
        ).arg(_shared->initialized() ? "true" : "false"));


make project:

    configure.bat x64 -D TDESKTOP_API_ID=123456 -D TDESKTOP_API_HASH=1234567890 -D DESKTOP_APP_USE_PACKAGED=OFF -D DESKTOP_APP_DISABLE_CRASH_REPORTS=OFF

modify dc_option:

    1. tdesktop\Telegram\SourceFiles\mtproto\mtproto_dc_options.cpp
        const BuiltInDc kBuiltInDcs[] = {
           { 1, "YOUR IP", PORT },
           { 2, "YOUR IP", PORT },
           { 3, "YOUR IP", PORT },
           { 4, "YOUR IP", PORT },
           { 5, "YOUR IP", PORT },
        };

        const BuiltInDc kBuiltInDcsTest[] = {
           { 1, "127.0.0.1", 12347 },
           { 2, "127.0.0.1", 12347 },
           { 3, "127.0.0.1", 12347 }
       };
    
       const char *kTestPublicRSAKeys[] = { "\
      -----BEGIN RSA PUBLIC KEY-----\n\
      MIIBCgKCAQEArPiEA6JTgNjNPjzSErNFMwANLcdkD+aU8pTjp620bMBzUV4aWJmR\n\
      lA2oddA4jc9Lej0Y6pvGZwWVl2lsMXB1Fe1P6YiTjj2Iz5X+gfLmbkiqZc6mxon+\n\
      QrYK87qPBwJ8FzBVBg0LxVwMsNj7uXv1wlCXDeM8ANXMULLa6SZe3QL1mG0Ys2+R\n\
      NXns3EnLKeukHWSofYh3jMDNe2zw4xlxSROP1HXF7OMZ2S4CKDy9jcecq+CHVAxb\n\
      T+pJp0mmV6vzGkaP+VP3Gg8c+FraWJrB8FcYtDGRHCLwL5ubArQeTufWs+56qqDO\n\
      1Lx1CK42dFlTopPrAj3Evb5MATmU9ggl8wIDAQAB\n\
      -----END RSA PUBLIC KEY-----" };
 
     const char *kPublicRSAKeys[] = { "\
     -----BEGIN RSA PUBLIC KEY-----\n\
     MIIBCgKCAQEArPiEA6JTgNjNPjzSErNFMwANLcdkD+aU8pTjp620bMBzUV4aWJmR\n\
     lA2oddA4jc9Lej0Y6pvGZwWVl2lsMXB1Fe1P6YiTjj2Iz5X+gfLmbkiqZc6mxon+\n\
     QrYK87qPBwJ8FzBVBg0LxVwMsNj7uXv1wlCXDeM8ANXMULLa6SZe3QL1mG0Ys2+R\n\
     NXns3EnLKeukHWSofYh3jMDNe2zw4xlxSROP1HXF7OMZ2S4CKDy9jcecq+CHVAxb\n\
     T+pJp0mmV6vzGkaP+VP3Gg8c+FraWJrB8FcYtDGRHCLwL5ubArQeTufWs+56qqDO\n\
     1Lx1CK42dFlTopPrAj3Evb5MATmU9ggl8wIDAQAB\n\
     -----END RSA PUBLIC KEY-----" };


build project:

    cd tdesktop\out
    msbuild Telegram.sln /property:Configuration=Debug /property:platform="x64"
    msbuild Telegram.sln /property:Configuration=Release /property:platform="x64"
