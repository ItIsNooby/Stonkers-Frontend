<html>
<head>
    <title>User Registration and Login</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script>$(document).ready(function() {$("#register-form").submit(function(e) {
                e.preventDefault();
                var username = $("#username").val();
                var email = $("#email").val();
                var password = $("#password").val();$.ajax({
                    url: "/register",
                    type: "POST",
                    data: {
                        username: username,
                        email: email,
                        password: password
                    },
                    success: function(response) {
                        alert(response);$("#register-form")[0].reset();
                    },
                    error: function(error) {
                        alert(error.responseText);
                    }
                });
            });$("#login-form").submit(function(e) {
                e.preventDefault();
                var usernameEmail = $("#username-email").val();
                var password = $("#password").val();                $.ajax({
                    url: "/login",
                    type: "POST",
                    data: {
                        username_email: usernameEmail,
                        password: password
                    },
                    success: function() {
                        window.location.href = "/dashboard";
                    },
                    error: function(error) {
                        alert(error.responseText);
                    }
                });
            });
        });
    </script>
</head>
<body>
    <h2>Register</h2>
    <form id="register-form">
        <label for="username">Username:</label>
        <input type="text" id="username" name="username" required><br><br>
        <label for="email">Email:</label>
        <input type="email" id="email" name="email" required><br><br>
        <label for="password">Password:</label>
        <input type="password" id="password" name="password" required><br><br>
        <input type="submit" value="Register">
    </form>
    <h2>Login</h2>
    <form id="login-form">
        <label for="username-email">Username/Email:</label>
        <input type="text" id="username-email" name="username-email" required><br><br>
        <label for="password">Password:</label>
        <input type="password" id="password" name="password" required><br><br>
        <input type="submit" value="Login">
    </form>
</body>
</html>