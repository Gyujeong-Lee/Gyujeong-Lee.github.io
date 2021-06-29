# 산출물
- Git hub Page & Action 

  - vue project 생성 
    - vue create project_name
    - Menually Select the features
    - 이후 아래 옵션들 선택합니다.
    - Check the features : Unit testing (필수)
    - Choose the version : 3.x
    - Pick a Linter  / Formatter config : ESLinter + Prettier
    - Pick a additional lint features :  Lint on save
    - Where do you  prefer placing config for Barbel, ESLint, etc
    - Save this as a preset for future project : N
  - yarn serve
  - 로컬 동작 확인

  ![image-20210629143510627](README.assets/image-20210629143510627.png)

  - GitHub Pages로 배포하기 위한 라이브러리 추가

    - `yarn add gh-pages -D`

  - Package.json에 배포에 필요한 정보 추가

    - ` "homepage": "https://github.com/Gyujeong-Lee/Gyujeong-Lee.github.io.git",`

    - ```
        "scripts": {
          "serve": "vue-cli-service serve",
          "build": "vue-cli-service build",
          "predeploy": "vue-cli-service build",
          "deploy": "gh-pages -d dist",
          "clean": "gh-pages-clean",
          "test:unit": "vue-cli-service test:unit",
          "lint": "vue-cli-service lint",
        },
      ```

  - 배포용 Path 설정

    - <Username>.github.io인 경우 필요 x 

    - 그 외 다른 경로로 지정시 필요

    - vue.config.js 추가

      ```
      module.exports = {
        publicPath: "/vue-devops",
        outputDir: "dist"
      };
      ```

  - Deploy
  
    - `yarn deploy`
  
  - GitHub Action을 통한 배포 자동화
  
    - set up this workflow
  
    - commit new file
  
    - <deploy>.yml 수정
  
      ```
      jobs:
        # This workflow contains a single job called "build"
        build:
          # The type of runner that the job will run on
          runs-on: ubuntu-latest
      
          # Steps represent a sequence of tasks that will be executed as part of the job
          steps:
            # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
            - uses: actions/checkout@v2
      
            # Runs a single command using the runners shell
            - name: Run a one-line script
              run: echo Hello, world!
      
            # Runs a set of commands using the runners shell
            - name: Run a multi-line script
              run: |
                echo Add other actions to build,
                echo test, and deploy your project.
      # deploy를 위한 코드
        deploy:
          runs-on: ubuntu-latest
      
          steps:
          - name: Checkout source code
            uses: actions/checkout@master
      
          - name: Set up Node.js
            uses: actions/setup-node@master
            with:
              node-version: 14.x
      
          - name: Istall dependencies
            run: yarn install
          
          - name: Test Unit
            run: yarn test:unit
            
          - name: Build Pages
            run: yearn build
            env:
              NODE_ENV: production
      
          - name: Deploy to gh-pages
            uses: peaceiris/action-gh-pages@v3
            with: 
              github_token: ${{ secrets.GITHUB_TOKEN}}
              publish_dir: ./dist 
      
      ```
  
      

