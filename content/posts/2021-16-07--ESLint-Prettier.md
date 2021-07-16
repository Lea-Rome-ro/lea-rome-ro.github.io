---
title: ESLint와 Prettier
date: "2021-07-15T22:35:32.169Z"
template: "post"
draft: false
slug: "ESLint-Prettier"
category: "React"
tags:
  - "React"
  - "Eslint"
  - "Prettier"
description: "ESLint와 Prettier 두가지를 비교하면서 두개의 개념에 대해 알아볼 수 있습니다. 또한, ESLint와 Prettier를 함께 사용하는 방법에 대해서도 간단히 설명해 보았습니다."
socialImage: "/media/React-icon.png"
---


## 1. ESLint개념 및 설치
![ESLint](/media/eslint.jpg)

###✔  ESLint란?
* ESLint는   __ES + Lint__ 의 합성어로 __코드의 포맷팅과 품질관리__ 도구이다.   
* ES는 EcmaScript(표준화된 스크립트 프로그래밍 언어)를 의미하고, Lint는 보푸라기라는 뜻으로 프로그래밍에서 에러가 있는 코드에 표시를 달아놓는것을 의미한다.  

  ESLint는 JS의 스타일 가이드를 따르지 않거나 문제가 있는 안티패턴들을 찾아주고, 일관된 코드스타일을 가질 수 있도록 도와주는 역할을 한다.  

###✔  ESLint 설치 및 설정

  - vscode와 node.js를 다운받아 설치한다.
  - terminal에서 `npm init`과 `npm install`을 실행하여 설치한다.
  - `yarn global add eslint` 혹은 `npm install -g eslint` 명령어를 사용하여 설치한다.
  - `eslint --init` 명령어로 초기화 해준 다음 다양한 설정값을 필요한 환경에 맞게 선택해준다. 
  ex)  
  *- ? How would you like to use ESLint?
    ❯ To check syntax and find problems  
? What type of modules does your project use?  
    ❯ None of these  
? Which framework does your project use?  
    ❯ None of these  
? Does your project use TypeScript?
    ❯ No  
? Where does your code run?  
    ❯ browser  
? What format do you want your config file to be in?  
    ❯ JavaScript*  

## 2. Prettier개념 및 설치
![Prettier](/media/prettier.jpg)

###✔  Prettier란?
* Prettier는 코드에 적용되어있던 스타일을 무시하고, 설정되어있는 정해진 규칙에 따라 **자동으로 코드 스타일을 정리해 주는 코드 포멧터**이다.
* **코드 포멧터** ⇒ 개발자가 작성한 코드를 정해진 코딩 스타일을 따르도록 변환해주는 도구   
* 코드의 한줄 최대 길이나, 탭의 길이, 따옴표 종류 등 **<u>코딩 스타일을 일괄적으로 통일</u>**해준다는 점에서, 특정 문법 요소를 사용하도록 해서 코드 퀄리티를 높여주는 ESLint와 차이점을 가지고 있다.  

###✔  Prettier 설치

  - `npm i prettier -D -E` 명령어를 사용하여 Prettier를 설치한다.  
     ( -E는 --save-exac의 축약형이다.)
  - `npx prettier test.js` 명령어로 Prettier를 실행한다. 
  - 위 명령을 수행할 때 코드 자체를 수정하고 싶다면 --write옵션을 추가하여 `npx prettier --write test.js`와 같이 명령어를 사용해 준다.  
  
## 3. 리엑트에 ESLint와 Prettier 적용하기  
<br/>

###✔ 리엑트 프로젝트 생성

 터미널에서 아래 명령어를 수행하여 리엑트 프로젝트를 생성해 준다.  
  명령어 : `create-react-app 프로젝트이름 --use-npm`  
<br/>

###✔ Prettier와 ESLint를 같이 사용하려면?

 __eslint-config-prettier__ 와 __eslint-plugin-prettier__ 모듈을 추가해줘야 한다.  
  명령어 : `npm i eslint-plugin-prettier eslint-config-prettier -D`  

 그 다음 **VSCode의 Extensions: Marketplace**로 들어가서 ESLint와 Prettier를 검색하여 설치한다.  
 <br/>

###✔ 설정파일 수정

  ♧  settings.json 파일을 열어 아래처럼 VSCode설정을 해준다.
 ```
{
// Set the default  
"editor.formatOnSave": false,  
// Enable per-language  
"[javascript]": {  
  "editor.formatOnSave": true  
},  
"editor.codeActionsOnSave": {  
  // For ESLint  
  "source.fixAll.eslint": true  
}  
}
```  

♧  터미널에서 아래 명령어를 수행하여 Airbnb Style Guide를 적용해준다.  
명령어 : `npm i -D eslint-config-airbnb` 

♧  그 다음 __.eslintrc.json__ 파일의 "extends"속성에 "airbnb"를 추가 해준다.  
 ```
{
"plugins": ["prettier"],
"extends": ["eslint:recommended", "plugin:prettier/recommended","airbnb"],
"rules": {
"prettier/prettier": "error"
}
}
 ```

♧  Prettier또한 ESLint와 마찬가지로 루트 경로에 __.prettierrc.json__ 파일을 만들어 주고, 필요한 옵션을 골라 설정해 준다.  
 ```
ex)
{
  "trailingComma": "es5",
  "tabWidth": 2,
  "semi": true,
  "singleQuote": true
}
 ```
 

## 추가 참고

https://sunmon.github.io/vscode-eslint-prettier-setting/  
https://velog.io/@recordboy/ESLint-Prettier-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0  
https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=dilrong&logNo=221413095305
