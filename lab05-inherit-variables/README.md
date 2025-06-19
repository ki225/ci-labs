# 05-inherit-variables
Inherit variable，變數傳承


## Run pipeline

- 使用上一個 lab 的方式 <br>
    ```yml
    job1:
        stage: level1
        artifacts:
            name: "level-1a-awards"
            when: on_success
            expire_in: "1 hrs"
            paths: 
            - ${VAR_ARTIFACTS}
            ...
    ```
  - artifacts + dependencies 的方式在接收方 job 所取得的是 artifacts 的原檔案，因此須先透過 `source` 指令去執行檔案讀取變數設定（也可以透過這個方法在檔案中放一些 shell 指令一起執行）<br>
    ```yml
    job2:
        ...
        script:
            - source ${VAR_ARTIFACTS}
    ```
- 使用特殊 `artifacts dotenv` 類型的方式進行傳承 <br>
    ```yml
    job1:
        stage: level1
        artifacts:
            name: ""dotenv
            when: on_success
            expire_in: "1 hrs"
            reports:
                detenv:
                    - ${_VARS_DOTENV}
            ...
    ```
  - artifacts dotenv 的方式檔案中的變數會在執行 job 時被建立，不須額外透過指令去讀取 <br>


## Some notes
- 一般 artifacts 在 pipeline 的或是 job 的頁面都可以進行下載 <br>
- dotenv 類型 artifacts 只可在 pipeline 頁面進行下載 <br>
- `>`是覆蓋寫入（overwrite），`>>`是追加寫入（append）
  ```
  echo "Line 1" > result.txt   # result.txt 內容為：Line 1
  echo "Line 2" >> result.txt  # 追加 → result.txt 內容為：
                                # Line 1
                                # Line 2
  ```

### keywords
- `artifacts:reports` : 特殊的 artifacts 類型 <br>
  ref: [artifacts:reports](https://docs.gitlab.com/ee/ci/yaml/#artifactsreports)
  - `artifacts:reports:dotenv` : 變數類型的特殊 artifacts <br>
    ref: [artifacts:reports:dotenv](https://docs.gitlab.com/ee/ci/yaml/artifacts_reports.html#artifactsreportsdotenv)


## 練習
在自己的 GitLab 空間中新增一個 project 進行練習

### 欲達成項目
1. pipeline 在使用瀏覽器 GUI 手動觸發時才會執行
1. 各 job 都使用 `basic` tag runner 即可
1. 設定兩個 stages，各 stage 中皆撰寫一個 job（stage 名稱自訂）

### job 執行內容
#### stage1 jobs
1. job 要產生一個隨機亂數存在 `_RANDOM_NUM` 變數中，亂數數值需在 0~100 的範圍內
1. job 要再產生當前時間存在 `_CURRENT_DATETIME` 變數中，格式為 `YYYY-mm-ddTHH:MM:SS`
1. job 要產生一個一般 artifact 用於 stage2 的 job
    - artifact 中儲存 `_CURRENT_DATETIME` 的值
    - artifact 中另外撰寫指令，要在 stage2 時執行該指令印出執行時的時間
1. job 也要產生一個 dotenv artifact 用於 stage2 的 job
    - artifact 中儲存 `_RANDOM_NUM` 的值
#### stage2 jobs
1. job 要取得 stage1 的 artifacts 並執行
    - 執行時應該要印出執行時的時間，格式為 `YYYY-mm-ddTHH:MM:SS`
    - 執行完後應該要讀取到 stage1 所設定 `_CURRENT_DATETIME` 的值，然後在 job 中印出該數值
1. job 要讀取 stage1 dotenv artifact 中所設定 `_RANDOM_NUM` 的值，然後在 job 中印出該數值
