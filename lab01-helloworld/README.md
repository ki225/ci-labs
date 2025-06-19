# 01-helloworld, 第一個 ci 

### Keywords
- `workflow` : 給 pipeline 一個名稱，較容易辨識執行內容（若沒設定會顯示最後的 commit message）<br>
  ref: [workflow](https://docs.gitlab.com/ee/ci/yaml/#workflow)
- `job` : pipeline（CI 的執行流程）中的基本組成，一個 pipeline 可以有多個 jobs 存在 <br>
  job 名稱可自行定義，此處命名為 `helloWorld` <br>
  ref: [jobs](https://docs.gitlab.com/ee/ci/jobs/index.html)
- `stage` : 指定 job 的執行階段，pipeline 有幾個預設的 stages [`.pre` `build` `test` `deploy` `.post`] <br>
  ref: [stage](https://docs.gitlab.com/ee/ci/yaml/#stage)
- `tags` : 指定要使用的 runner，會挑選帶有指定 tags 的 runner 來執行該 job <br>
  ref: [tags](https://docs.gitlab.com/ee/ci/yaml/#tags)
- `rules` : job 的執行條件，定義 job 是否會再目前 pipeline 觸發的條件中被執行 <br>
  ref: [rules](https://docs.gitlab.com/ee/ci/yaml/#rules)
  - `rules:if` : 定義將 job 加入 pipeline 中的情境 <br>
    ref: [rules:if](https://docs.gitlab.com/ee/ci/yaml/#rulesif)
- `script` : job 的主要執行內容 (以 bash 腳本進行撰寫) <br>
  ref: [script](https://docs.gitlab.com/ee/ci/yaml/#script)


## 練習
在自己的 GitLab 空間中新增一個 project 進行練習

### 欲達成項目
1. 包含一個 job (名稱自訂)
2. job 使用 `basic` tag 的 runner 執行
3. 把這個 job 放在 stage `practice` 執行（自定義 stage）
4. pipeline 在使用瀏覽器 GUI 手動觸發時才會執行
  ![manual pipeline trigger](images/trigger-pipeline.png)

### job 執行內容
1. 在 console 目前印出所在路徑
2. 列出目前路徑中的檔案 / 資料夾
3. 建立一個資料夾 `practice-01`
4. 在上一步所建立的資料夾 `practice-01` 中建立一個檔案 `hello.txt`
5. 在檔案 `practice-01/hello.txt` 輸入內容 `hello world`
6. 在 console 印出 `practice-01/hello.txt` 的內容