<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Air 1102 Forum</title>
    <!-- Ganti tautan Bootstrap dengan Bootstrap 5 -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-alpha1/dist/css/bootstrap.min.css">
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.1/font/bootstrap-icons.css">
    <style>
        body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #f0f2f5;
    margin: 0;
    padding: 0;
}
        /* Tambahkan CSS untuk form post */
        .post-form {
            background-color: #f5f6f7;
            border: 1px solid #ccc;
            border-radius: 5px;
            padding: 10px;
            margin-bottom: 20px;
        }

        .posts-list {
            padding-top: 20px;
        }

        .post {
            margin-bottom: 20px;
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.1);
        }

        .footer {
            background-color: #f8f9fa;
            padding: 20px 0;
            text-align: center;
        }

        /* Navigasi responsif */
        @media (max-width: 768px) {
            .navbar-nav {
                flex-direction: column;
            }

            .nav-item {
                margin-bottom: 10px;
            }
        }

        .post-time {
            color: #555;
            font-size: 12px;
        }
        
            /* Navigasi responsif */
        @media (max-width: 768px) {
            .navbar-collapse {
                flex-basis: 100%;
                justify-content: center;
            }

            .navbar-nav {
                flex-direction: column;
                text-align: center;
            }

            .navbar-toggler {
                order: -1;
            }
        }
    </style>
</head>

<body>

   <ul class="nav nav-tabs" id="myTab" role="tablist">
  <li class="nav-item" role="presentation">
    <button class="nav-link active" id="home-tab" data-bs-toggle="tab" data-bs-target="#home" type="button" role="tab" aria-controls="home" aria-selected="true">Home</button>
  </li>
  <li class="nav-item" role="presentation">
    <button class="nav-link" id="profile-tab" data-bs-toggle="tab" data-bs-target="#profile" type="button" role="tab" aria-controls="profile" aria-selected="false">Profile</button>
  </li>
  <li class="nav-item" role="presentation">
    <button class="nav-link" id="contact-tab" data-bs-toggle="tab" data-bs-target="#contact" type="button" role="tab" aria-controls="contact" aria-selected="false">Contact</button>
  </li>
</ul>
<div class="tab-content text-center 1rem" id="myTabContent">
  <div class="tab-pane fade show active" id="home" role="tabpanel" aria-labelledby="home-tab"></div>
  <div class="tab-pane fade" id="profile" role="tabpanel" aria-labelledby="profile-tab"><a href="http://" class="bi bi-book-half display-7 fst-italic"> DOCS</a></div>
  <div class="tab-pane fade" id="contact" role="tabpanel" aria-labelledby="contact-tab"><a class="bi bi-instagram" href="http://"></a> <a class="bi bi-tiktok" href="http://"></a></div>
</div>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-8 offset-md-2">
                <!-- Form posting -->
                <div class="post-form">
                    <textarea class="form-control mb-3" id="post-text"
                        placeholder="Apa yang sedang Anda pikirkan?"></textarea>
                    <button class="btn btn-success" id="post-button">Post</button>
                </div>

                <!-- Daftar posting -->
                <div class="posts-list"></div>
            </div>
        </div>
    </div>

    <div class="footer">
        <p>&copy; 2023 @Air 1102. All rights reserved.</p>
    </div>

    <!-- Ganti skrip Bootstrap dengan Bootstrap 5 -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-alpha1/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        // Mendapatkan elemen-elemen DOM
        const postText = document.getElementById("post-text");
        const postButton = document.getElementById("post-button");
        const postsList = document.querySelector(".posts-list");

        // Mendapatkan data dari localStorage saat halaman dimuat
        let posts = JSON.parse(localStorage.getItem("posts")) || [];

        // Fungsi untuk merender ulang posting dengan format waktu yang diinginkan
        function renderPosts() {
            postsList.innerHTML = "";
            posts.forEach(post => {
                const postElement = document.createElement("div");
                postElement.className = "post";

                // Format waktu
                const postTime = new Date(post.id);
                const formattedTime = calculateTime(postTime);

                postElement.innerHTML = `
                    <p>${post.text}</p>
                    <p class="post-time">Posted ${formattedTime} ago</p>
                    <div class="post-actions">
                        <button class="btn btn-primary" onclick="likePost(${post.id})">👍 Like (${post.likes})</button>
                        <button class="btn btn-warning" onclick="editPost(${post.id})">✏️ Edit</button>
                        <button class="btn btn-danger" onclick="deletePost(${post.id})">🗑️ Delete</button>
                    </div>
                `;
                postsList.appendChild(postElement);
            });
        }

        // Fungsi untuk menghitung waktu yang lalu
        function calculateTime(postTime) {
            const now = new Date();
            const differenceInSeconds = Math.floor((now - postTime) / 1000);
            if (differenceInSeconds < 60) {
                return `${differenceInSeconds} seconds`;
            } else if (differenceInSeconds < 3600) {
                const minutes = Math.floor(differenceInSeconds / 60);
                return `${minutes} minutes`;
            } else if (differenceInSeconds < 86400) {
                const hours = Math.floor(differenceInSeconds / 3600);
                return `${hours} hours`;
            } else {
                const days = Math.floor(differenceInSeconds / 86400);
                return `${days} days`;
            }
        }

        // Fungsi untuk memposting
        function submitPost() {
            const postTextValue = postText.value.trim();
            if (postTextValue === "") {
                alert("Silakan tulis sesuatu sebelum memposting!");
                return;
            }

            const post = {
                text: postTextValue,
                id: new Date().getTime(),
                likes: 0
            };

            posts.push(post);
            // Menyimpan data ke localStorage
            localStorage.setItem("posts", JSON.stringify(posts));

            // Merender ulang posting
            renderPosts();
            // Mengosongkan input teks
            postText.value = "";
        }

        // Fungsi untuk menyukai posting
        function likePost(postId) {
            const post = posts.find(post => post.id === postId);
            if (post) {
                post.likes++;
                // Menyimpan data ke localStorage
                localStorage.setItem("posts", JSON.stringify(posts));
                // Merender ulang posting
                renderPosts();
            }
        }

        // Fungsi untuk mengedit posting
        function editPost(postId) {
            const post = posts.find(post => post.id === postId);
            if (post) {
                const updatedText = prompt("Edit postingan:", post.text);
                if (updatedText !== null) {
                    post.text = updatedText;
                    // Menyimpan data ke localStorage
                    localStorage.setItem("posts", JSON.stringify(posts));
                    // Merender ulang posting
                    renderPosts();
                }
            }
        }

        // Fungsi untuk menghapus posting
        function deletePost(postId) {
            const confirmed = confirm("Apakah Anda yakin ingin menghapus posting ini?");
            if (confirmed) {
                const postIndex = posts.findIndex(post => post.id === postId);
                if (postIndex !== -1) {
                    posts.splice(postIndex, 1);
                    // Menyimpan data ke localStorage
                    localStorage.setItem("posts", JSON.stringify(posts));
                    // Merender ulang posting
                    renderPosts();
                }
            }
        }

        // Mendengarkan klik tombol "Post"
        postButton.addEventListener("click", submitPost);

        // Merender ulang posting saat halaman dimuat
        renderPosts();
    </script>
    <script src="script.js"></script>
    <script src="path/to/bootstrap.bundle.min.js"></script>
     <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.2/dist/umd/popper.min.js" integrity="sha384-IQsoLXl5PILFhosVNubq5LC7Qb9DXgDA9i+tQ8Zj3iwWAwPtgFTxbJ8NT4GN1R8p" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.min.js" integrity="sha384-cVKIPhGWiC2Al4u+LWgxfKTRIcfu0JTxR+EQDz/bgldoEyl4H0zUF0QKbrJ0EcQF" crossorigin="anonymous"></script>
    
</body>

</html>
