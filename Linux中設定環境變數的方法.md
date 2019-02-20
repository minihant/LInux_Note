# Linux中設定環境變數的方法
## 當你需要作cross-compiler時，須要先安裝Toolchain在你的工作電腦上(Ubuntu, Fedora, Debiab etc.), 與其說安裝其實就是把Toolchain包解壓縮到某個路徑。接著要作交叉編譯時，需要指定編譯工具的路徑，此時為了不用每次都輸入一整串的路徑來指定編譯工具，就會去設定PATH。例如:我的編譯器arm-linux-gnueabihf-gcc路徑是在 "home/danny/workspace/DB410C/Toolchain/gcc-linaro-4.9-2014.11-x86_64_arm-linux-gnueabihf/bin"，此時有三個方法來設定環境變數:
    1. 用export指令
    ```
    export PATH=$PATH:/home/danny/workspace/DB410C/Toolchain/gcc-linaro-4.9-2014.11-x86_64_arm-linux-gnueabihf/bin
        輸入之後可以使用export指令來查看環境變數是否有輸入進去。
        此修改重開機後，就必須再作一次
    ```
    2. 修改profile
        - profile的路徑是在 "/etc/profile" , 直接修改profile這個檔案在裡面加入
        ```
        export PATH=$PATH:/home/danny/workspace/DB410C/Toolchain/gcc-linaro-4.9-2014.11-x86_64_arm-linux-gnueabihf/bin
        此修改必須在重開機之後，才會有作用    
        ```
    3. 修改.bashrc 
        - .bashrc的路徑是在"/home/danny/.bashrc" 在檔案最後面加入:
        ```
        export PATH=$PATH:/home/danny/workspace/DB410C/Toolchain/gcc-linaro-4.9-2014.11-x86_64_arm-linux-gnueabihf/bin
            *此修改只需關掉Terminal在開啟後，就都會被設定
        ```
    4. 另外發現也可以直接去修改 /etc/enviroment
        - 這檔案裡面包含原本PATH變數的資料, 要增加請在最後面用:加上你要加入的路徑即可, 例如:
        ```
        PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/home/danny/workspace/DB410C/Toolchain/gcc-linaro-4.9-2014.11-x86_64_arm-linux-gnueabihf/bin"
        ```

# Linux VisualBox 分享資料夾方法
    ```
    sodo mount –t vboxsf sourceDir DesDir
    ```