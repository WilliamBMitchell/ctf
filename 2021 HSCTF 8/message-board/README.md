## Problem ##

Your employer, LameCompany, has lots of gossip on its company message board: message-board.hsc.tf. You, Kupatergent, are able to access some of the tea, but not all of it! Unsatisfied, you figure that the admin user must have access to ALL of the tea. Your goal is to find the tea you've been missing out on.

Your login credentials: username: `kupatergent` password: `gandal`

Server code is attached (slightly modified).

## Solution ##

```console
root@osboxes:~/Downloads/message-board-1-master# cat app.js 
const express = require("express")
const cookieParser = require("cookie-parser")
const ejs = require("ejs")
require("dotenv").config()

const app = express()
app.use(express.urlencoded({ extended: true }))
app.use(cookieParser())
app.set("view engine", "ejs")
app.use(express.static("public"))

const users = [
    {
        userID: "972",
        username: "kupatergent",
        password: "gandal"
    },
    {
        userID: "***",
        username: "admin"
    }
]
```


```console
0
1
2
.
.
.
763
764
765
766
767
768
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LameCompany: Message Board</title>

    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-+0n0xVW2eSR5OomGNYDnhzAbDsOXxcvSN1TPprVMTNDbiYZCxYbOOl7+AMvyTG2x" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.1/dist/js/bootstrap.bundle.min.js" integrity="sha384-gtEjrD/SeCtmISkJkNUaaKMoLD0//ElJ19smozuHV6z3Iehds+3Ulb9Bn9Plx0x4" crossorigin="anonymous"></script>

    <link rel="stylesheet" href="styles.css">
</head>
<body>
     

<div class="container mid">
    <h1>hi, admin</h1>

    <div class="messages">
        <p><strong data-bs-toggle="tooltip" title="a super friendly person :)">Karen:</strong> Did you see Mary's hair the other day?</p>
        <p><strong data-bs-toggle="tooltip" title="had terrible hair the other day">Mary:</strong> I can hear you, you know.</p>
        <p><strong data-bs-toggle="tooltip" title="a super friendly person :)">Karen:</strong> Technically you're not hearing me.</p>
        <p><strong data-bs-toggle="tooltip" title="has had enough of this bs">Rosa:</strong> Okay, I'm heading out.</p>
        <p><strong data-bs-toggle="tooltip" title="what a cool name">HSCTF:</strong> flag{y4m_y4m_c00k13s}</p>
    </div>
</div>

<a href="/logout"><button class="btn btn-danger">Log out</button></a>

<script>
    var tooltipTriggerList = [].slice.call(document.querySelectorAll('[data-bs-toggle="tooltip"]'))
    var tooltipList = tooltipTriggerList.map(function (tooltipTriggerEl) {
        return new bootstrap.Tooltip(tooltipTriggerEl)
    })
</script>


</body>
</html> 
```

The flag is **flag{y4m_y4m_c00k13s}**.
