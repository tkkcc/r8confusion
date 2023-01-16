# R8 confusion on dependencies

R8 is meant to shrink unneeded code, thus just adding an unused dependency must not increase output size.

But it's wrong.

## To Reproduce

1. New project in Android Studio, choose first No Activity template.

1. set `minifyEnabled` and `shrinkResources` to `true`, switch build varient to `release`, then build. output size is
    ```sh
    du -sh app/build/intermediates/dex/release/minifyReleaseWithR8/classes.dex
    1.3M	app/build/intermediates/dex/release/minifyReleaseWithR8/classes.dex
    du -sh app/build/outputs/apk/release/app-release-unsigned.apk
    1.7M
    ```

1. add `androidx.compose.material3:material3:1.0.1` dependency, build again. output size is
    ```sh
    du -sh app/build/intermediates/dex/release/minifyReleaseWithR8/classes.dex
    1.6M	app/build/intermediates/dex/release/minifyReleaseWithR8/classes.dex
    du -sh app/build/outputs/apk/release/app-release-unsigned.apk
    1.9M
    ```

1. add `com.google.android.gms:play-services-mlkit-text-recognition:18.0.2` dependency, build again. output size is
    ```sh
    du -sh app/build/intermediates/dex/release/minifyReleaseWithR8/classes.dex
    1.8M	app/build/intermediates/dex/release/minifyReleaseWithR8/classes.dex
    du -sh app/build/outputs/apk/release/app-release-unsigned.apk
    2.2M
    ```
