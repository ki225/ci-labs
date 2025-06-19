# When to run, 控制什麼時候做什麼事


### keywords
- `rules` : job 的執行條件，定義 job 是否會再目前 pipeline 觸發的條件中被執行 <br>
  ref: [rules](https://docs.gitlab.com/ee/ci/yaml/#rules)
  - `rules:if` : 定義將 job 加入 pipeline 中的情境 <br>
    ref: [rules:if](https://docs.gitlab.com/ee/ci/yaml/#rulesif)
  - `rules:variables` : 根據符合的條件設定自定義變數 <br>
    ref: [rules:variables](https://docs.gitlab.com/ee/ci/yaml/#rulesvariables)
- `when` : job 的執行時機，預設為當前面 stages 的 jobs 都執行成功時會執行 (`on_success`) <br>
  ref: [when](https://docs.gitlab.com/ee/ci/yaml/#when)

## Some notes
- pipeline 中有許多關於 pipeline / project 的預定義變數可以讀取 


## 練習
在自己的 GitLab 空間中新增一個 project 進行練習

### 欲達成項目
1. pipeline 在使用瀏覽器 GUI 手動觸發時才會執行
2. 各 job 都使用 `basic` tag runner 即可
3. 兩個 jobs

### job 執行內容
- 其中一個 job 在 console 印出最近一次的 git commit message 跟 git commit sha
- 一個 job 只有在觸發 pipeline 時給變數 `_EASTER_EGG` 才會執行
  ![manual input variable](images/manual-input-var.png)
    - 當 `_EASTER_EGG` 為 `Halloween` 時，要在 console 印出訊息：
      ```
      Trick or Treat!
      ```  
    - 當 `_EASTER_EGG` 為 `Christmas` 時，要在 console 印出訊息：
      ```
      Santa Claus is Coming to Town!
      ```
    - 當 `_EASTER_EGG` 不是上面兩個值時，在 console 印出訊息：
      ```
      BROKEN EGG
      ```