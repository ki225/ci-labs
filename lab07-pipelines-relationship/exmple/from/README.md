# trigger-from
pipelines-relationship (trigger from), 起點


## Run pipeline


- trigger 會攜帶既有設定的 variables 變數去觸發所設定的 `project` repository <br>
    ```
    trigger:
        stage: trigger
        variables:
            _VERSION: $_VERSION
            _SRC: 'trigger-from'
        rules:
            - if: '$CI_PIPELINE_SOURCE == "web"'
            when: manual
        trigger:
            project: '/xxxx/07-pipelines-relationship/trigger-to'
            strategy: depend
            branch: main
    ```
  - 被觸發的 pipeline 會依照觸發所帶的變數執行有符合條件的 jobs
- 透過 trigger 觸發會在 pipeline 頁面出現 Downstream 區塊，可直接透過該區塊查看後續 pipeline 執行狀況 <br>
- `release-package` job 經由 [GitLab REST API](https://docs.gitlab.com/ee/api/api_resources.html) 將資料上傳到 GitLab Package Registry 中，可供後續下載使用 <br>
  Package Registry 可從側邊欄 `Deploy -> Package registry` 中查看 <br>
  Package format mapping (gui) <br>
  ![](../src/img/img6.png)
  ![](../src/img/img7.png)

## Some notes
- package 可透過[幾種方式](https://docs.gitlab.com/ee/user/packages/package_registry/index.html#authenticate-with-the-registry)進行權限驗證，這邊是使用 `CI_JOB_TOKEN` 的方式進行
- CI variables 優先複寫順序，當有重複定義時可參考 [ci variable precedence 文件](https://docs.gitlab.com/ee/ci/variables/#cicd-variable-precedence)
  - 沒有額外設定變數時會吃 CI 腳本中所設定 version value: `latest` <br>
  - 觸發時自行定義變數 `_VERSION`，會由該數值覆蓋腳本中預設 value -> `0.1.0` (Manual pipeline run variables > Variables defined outside of jobs [globally] in the .gitlab-ci.yml file) <br>

### keywords
- `trigger` : 對其他 repository CI 進行觸發 <br>
  ref: [trigger](https://docs.gitlab.com/ee/ci/yaml/#trigger)
- [publish generic package](https://docs.gitlab.com/ee/user/packages/generic_packages/index.html#publish-a-generic-package-by-using-cicd)
