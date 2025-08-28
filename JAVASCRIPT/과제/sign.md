#sign.html
```
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Document</title>
  <link rel="stylesheet" href="sign.css" />
</head>
<body>
  <div class="signup-form">
    <h2>회원가입</h2>

    <form id="signupForm" action="" novalidate>
      <label for="name">이름</label>
      <input type="text" id="name" name="name" placeholder="이름을 입력하세요.">
      <div id="nameError" class="error"></div>

      <label for="email">이메일</label>
      <input type="email" id="email" name="email" placeholder="이메일을 입력하세요.">
      <div id="emailError" class="error"></div>

      <label for="phone">전화번호</label>
      <input type="tel" id="phone" name="phone" placeholder="전화번호를 입력하세요.">
      <div id="phoneError" class="error"></div>

      <label for="address">주소</label>
      <input type="text" id="address" name="address" placeholder="주소를 입력하세요.">
      <div id="addressError" class="error"></div>

      <label for="password">비밀번호</label>
      <input type="password" id="password" name="password" placeholder="비밀번호를 입력하세요.">
      <div id="passwordError" class="error"></div>

      <label for="confirm_password">비밀번호 확인</label>
      <input type="password" id="confirm_password" name="confirm_password" placeholder="비밀번호를 다시 입력하세요.">
      <div id="confirmError" class="error"></div>

      <input type="submit" value="가입하기">
    </form>
  </div>

  <script>
    const form = document.getElementById('signupForm');

    const nameInput = document.getElementById('name');
    const emailInput = document.getElementById('email');
    const phoneInput = document.getElementById('phone');
    const addressInput = document.getElementById('address');
    const pwInput = document.getElementById('password');
    const pw2Input = document.getElementById('confirm_password');

    const nameError = document.getElementById('nameError');
    const emailError = document.getElementById('emailError');
    const phoneError = document.getElementById('phoneError');
    const addressError = document.getElementById('addressError');
    const pwError = document.getElementById('passwordError');
    const pw2Error = document.getElementById('confirmError');

    function clearErrors() {
      nameError.textContent = '';
      emailError.textContent = '';
      phoneError.textContent = '';
      addressError.textContent = '';
      pwError.textContent = '';
      pw2Error.textContent = '';
    }

    function checkPasswordsMatch() {
 
      if (pwInput.value === '' || pw2Input.value === '') {
        pw2Error.textContent = '';
        return true;
      }
      if (pwInput.value !== pw2Input.value) {
        pw2Error.textContent = '비밀번호가 일치하지 않습니다.';
        return false;
      }
      pw2Error.textContent = '';
      return true;
    }


    pwInput.addEventListener('input', checkPasswordsMatch);
    pw2Input.addEventListener('input', checkPasswordsMatch);

    form.addEventListener('submit', (e) => {
      e.preventDefault(); 

      let valid = true;
      clearErrors();

      if (nameInput.value.trim() === '') { nameError.textContent = '이름을 입력하세요.'; valid = false; }
      if (emailInput.value.trim() === '') { emailError.textContent = '이메일을 입력하세요.'; valid = false; }
      if (phoneInput.value.trim() === '') { phoneError.textContent = '전화번호를 입력하세요.'; valid = false; }
      if (addressInput.value.trim() === '') { addressError.textContent = '주소를 입력하세요.'; valid = false; }
      if (pwInput.value.trim() === '') { pwError.textContent = '비밀번호를 입력하세요.'; valid = false; }
      if (!checkPasswordsMatch()) valid = false;

      if (valid) {
        alert('검증 통과! (백엔드 연동 전이라 전송은 하지 않았어요)');
      }
    });
  </script>
</body>
</html>

```
#sign.css
```
body {
    font-family: arial, sans-serif;
    background-color: #f2f2f2;
    padding: 40px;
}

.signup-form {
    width: 400px;
    margin: auto;
    background: white;
    padding: 30px;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.signup-form h2 {
    text-align: center;
    margin-bottom: 20px;
}

.signup-form label {
    display: block;
    margin-top: 10px;
}

.signup-form input[type="text"],
.signup-form input[type="email"],
.signup-form input[type="password"] {
    width: 100%;
    padding: 10px;
    margin-top: 5px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

.signup-form input[type="tel"] {  
  width: 100%;
  padding: 10px;
  margin-top: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.signup-form input[type="submit"] {
    width: 100%;
    padding: 10px;
    margin-top: 20px;
    background-color: #4caf50;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.signup-form input[type="submit"]:hover {
    background-color: #45a049;
}

#error {
    color: red;
    font-size: 0.9em;
}
```
