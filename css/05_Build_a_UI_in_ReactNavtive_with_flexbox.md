# build ui in react native with flexbox

## react-native 설치

[source](https://github.com/codeschool/WatchUsBuild-FlexboxReactNativeUI)

```
$ brew install node
$ brew install watchman
```

Xcode가 설치되어 있어야 함.

프로젝트가 위치한 폴더로 이동해서
```
$ npm install -g react-native-cli
$ npm install
```

Build 오류가 날 경우 react랑 react-native를 최신 버전으로 업그레이드

or

Xcode에서 Scheme을 Debug로 변경해줌.

```
$ npm install react@latest react-native@latest
```

8081 Port가 사용중일 경우

```
$ sudo lsof -n -i4TCP:8081 | grep LISTEN
$ kill -9 [number]
```

시뮬레이터 실행
```
$ react-native run-ios --simulator "iphone 7"
$ react-native run-ios
```

## Git Source 참고해서 만들어보기

### 웹 버전

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta http-equiv="x-ua-compatible" content="ie=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link href="https://fonts.googleapis.com/css?family=Oswald" rel="stylesheet">
    <link rel="stylesheet" type="text/css" href="styles/app.css">
</head>
<body class="landing">

    <main>

        <section class="signin">

            <img alt='Villainr character illustration' src='images/villainr-brand.svg' width='30%' />
            <h1 class="landing-title">日週月</h1>
            <p class="landing-slogan">Study Daily, Weekly, Monthly</p>
            <form>
                <fieldset>
                    <label for="username">Username</label>
                    <input type="text" id="username" placeholder="ID">
                </fieldset>
                <fieldset>
                    <label for="password">Password</label>
                    <input type="password" id="password" placeholder="password">
                </fieldset>
                <input  class="btn" type="submit" value="Sign In">
            </form>
        </section>

    </main>

</body>
</html>
```

```html
<!-- review.html -->
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta http-equiv="x-ua-compatible" content="ie=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link href="https://fonts.googleapis.com/css?family=Oswald" rel="stylesheet">
    <link rel="stylesheet" type="text/css" href="styles/app.css">
</head>
<body class="interior">

    <header>
        <h1>日週月</h1>
    </header>

    <main>

        <section class="profile">

            <section class="profile-bio">
                <img class='profile-image' src='https://s3-us-west-2.amazonaws.com/s.cdpn.io/t-112/villain-profile.svg' width='250' />
                <h3 class="profile-name">Dude</h3>
                <p class="profile-description">This dude studied every Monday. But he forgot every Friday. Well done dude!</p>
            </section>

            <section class="profile-controls">
                <button class="control control-positive">Yay</button>
                <button class="control control-info">I</button>
                <button class="control control-negative">Nay</button>
            </section>

        </section>

    </main>

    <footer>
        <p class="disclaimer">&copy; TeamHalloween, 2016</p>
    </footer>
</body>
</html>
```

```css
/* ***************************************** */
/*                                           */
/*    App CSS                                */
/*    -> One file to style them all          */
/*                                           */
/* ***************************************** */

/* ***************************************** */
/* Colors                                    */
/* ***************************************** */

/* background brownish: #2a1a12 */
/* text: #99a6a7 */
/* text featured: #8c546d */


/* ***************************************** */
/* Global                                    */
/* ***************************************** */

* {
    box-sizing: border-box;
}

html {
    font-size: 16px;
}

html,
body {
    height: 100%;
}


body {
    font-size: 62.5%;
    justify-content: center;
    align-items: center;
    background: #2a1a12;
    color: #99a6a7;
    font-family:'Oswald', sans-serif;
    font-size: 18px;
    margin: 0;
    padding: 0;
    display: flex;
}

h1, h2, h3, h4, h5, h6 {
    color: #8c546d;
}

h1 {
    font-size: 2em;
}

/* ***************************************** */
/* Buttons                                   */
/* ***************************************** */

.btn {
    background: #a4d7d3;
    border-radius: 4px;
    padding: 12px 0;
    transition: background 0.25s linear
}

.btn:hover {
    background: #c7e7e4;
}

/* ***************************************** */
/* Forms                                     */
/* ***************************************** */

form {
    text-align: left;
}

input {
    border: 0;
    border-radius: 4px;
    display: block;
    font-family: 'Oswald', sans-serif;
    font-size: 1em;
    padding: 8px;
    width: 100%;
}

input[type='submit'] {
    display: block;
    margin: 18px auto 0 auto;
    width: 50%;
}

label {
    display: block;
    font-size: 1em;
    margin-bottom: 4px;
}

fieldset {
    border: 0;
    padding: 5px 0;
}

/* ***************************************** */
/* Landing                                   */
/* ***************************************** */

.signin {
    max-width: 380px;
    text-align: center;
}

.landing-title {
    line-height: 1;
    margin: 9px 0 0 0;
}

.landing-slogan {
    font-size: 1.2em;
    margin: 0 20px;
    margin-top: 9px;
}

/* ***************************************** */
/* Review                                    */
/* ***************************************** */

.interior {
    background: #f6f7f7;
    flex-direction: column;
    justify-content: space-between;
    align-items: stretch;
}

.interior header,
.interior footer {
    background: #281910;
    padding: 10px;
    text-align: center;
}

.interior main {
    align-self: center;
    overflow-y: scroll;
}

.interior h1 {
    margin: 0;
}

.disclaimer {
    margin: 0;
    font-size: 0.8em;
}

.profile {
    background: #ffffff;
    border-radius: 4px;
    border: 1px solid #e3e7e7;
    box-shadow: 0 1px 1px #edefef;
    overflow: hidden;
    padding: 20px;
}

.profile-name {
    text-align: center;
    color: #281910;
    font-weight: normal;
    margin: 0;
}

.profile-description {
    font-size: 0.8em;
    margin: 5px 0 0 0;
    max-width: 250px;
}

.profile-controls {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 20px;
}

/* ***************************************** */
/* Controls                                  */
/* ***************************************** */

.control {
    background: none;
    border: 1px solid #e3e7e7;
    border-radius: 50%;
    box-shadow: 0 1px 1px #edefef;
    font-size: 0;
    height: 70px;
    width: 70px;
}

.control + .control {
    margin-left: 15px;
}

.control-positive {
    background: url("../images/icon-positive.png") no-repeat center center / 35px 35px;
}

.control-info {
    background: url("../images/icon-information.png") no-repeat center center / 20px 20px;
    height: 40px;
    width: 40px;
}

.control-negative {
    background: url("../images/icon-negative.png") no-repeat center center / 35px 35px;
}
```

### React-native






































