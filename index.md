<html>
<head>
    <title>User Registration and Login</title>
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
    <script>
        document.getElementById("register-form").addEventListener("submit", function(e) {
            e.preventDefault();
            var username = document.getElementById("username").value;
            var email = document.getElementById("email").value;
            var password = document.getElementById("password").value;
            fetch("https://stonkers-backend.duckdns.org/register", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify({
                    username: username,
                    email: email,
                    password: password
                })
            })
            .then(response => response.text())
            .then(data => {
                alert(data);
                document.getElementById("register-form").reset();
            })
            .catch(error => {
                alert("An error occurred: " + error);
            });
        });
        document.getElementById("login-form").addEventListener("submit", function(e) {
            e.preventDefault();
            var usernameEmail = document.getElementById("username-email").value;
            var password = document.getElementById("password").value;
            fetch("https://stonkers-backend.duckdns.org/login", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify({
                    username_email: usernameEmail,
                    password: password
                })
            })
            .then(response => {
                if (response.ok) {
                    window.location.href = "/dashboard";
                } else {
                    throw new Error("Invalid credentials!");
                }
            })
            .catch(error => {
                alert("An error occurred: " + error);
            });
        });
    </script>
</body>
</html>