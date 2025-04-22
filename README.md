<?php
session_start();

// Inisialisasi array user jika belum ada
if (!isset($_SESSION['users'])) {
    $_SESSION['users'] = [];
}

// Menambah user 
if (isset($_POST['add_user'])) {
    $username = $_POST['username'];
    $password = $_POST['password'];
    $_SESSION['users'][] = ['username' => $username, 'password' => $password];
}

// Menhapus user
if (isset($_GET['delete'])) {
    $index = $_GET['delete'];
    unset($_SESSION['users'][$index]);
    $_SESSION['users'] = array_values($_SESSION['users']); // Reindex array
}

// Mengedit user
if (isset($_POST['edit_user'])) {
    $index = $_POST['index'];
    
    $_SESSION['users'][$index]['password'] = $_POST['password'];
}
?>

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viemport" content="width=device-width, initial-scale=1.0">
    <title>Manajemen User</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 0;
        }

        h2 {
            text-align: center;
        }

        .login-conatiner, .dashboard-container, .container {
            max-width: 400px;
            margin: 50px auto;
            padding: 20px;
            background-color: #fff;
            border-radius: 8px;
            box-shadow: 0 0 1px regba(0, 0, 0, 0.1);
        }

        .login-container form input,
        .login-conatiner form button {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border: 1px solid #ccc;
            border-radius: 4px;
        }

        .login-container form button {
            background-color: #4CAF50;
            color: white;
            border: none;
        }

        .error {
            color: red;
            text-align: center;
        }

        .menu {
            display: flex;
            flex-direction: column;
            gap: 10px;
            align-items: center;
        }

        .menu a {
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            text-decoration: none;
            border-radius: 4px;
        }

        .menu a:hover {
            background-color: #45a049;
        }

        .logout {
            display: block;
            text-align: center;
            padding: 10px;
            background-color: #f44336;
            color: white;
            text-decoration: none;
            border-radius: 4px;
            margin-top: 20px;
        }

        .logout:hover {
            background-color: #d32f2f;
        }

        .back {
            display: block;
            text-align: center;
            padding: 10px;
            background-color: #2196F3;
            color: white;
            text-decoration: none;
            border-radius: 4px;
            margin-top: 20px;
        }

        .back:hover {
            background-color: #1976D2;
        }
        </style>
        </html>
    </head>
    <body>
        <div class="dashboard-conatiner">
            <h2>Manajemen User</h2>

            <!-- Form untuk menambah user -->
             <form method="POST" action="">
                <input type="text" name="username" placeholder="Nama User" required>
                <input type="password" name="password" placeholder="Email User" required>
                <button type="submit" name="add_user">Tambah User</button>
    </form>

    <!-- Daftar User -->
     <h3>Daftar User</h3>
     <table>
        <thead>
            <tr>
                <th>Nama</th>
                <th>Email</th>
                <th>Aksi</th>
    </tr>
    </thead>
    <tbody>
        <?php foreach ($_SESSION['users'] as $index => $user): ?>
            <tr>
                <td><?php echo $user['username']; ?></td>
                <td><?php echo $user['password']; ?></td>
                <td>
                    <!-- Edit user -->
                     <a href="manajemen_user.php?edit=<?php echo $index; ?>">Edit</a>
                     <!-- Hapus user -->
                      <a href="manajemen_user.php?delete=<?php echo $index; ?>" onclick="return confirm('Apakah Anda yakin ingin menhapus?')">Hapus</a>
        </td>
        </tr>
        <?php endforeach; ?>
        </tbody>
        </table>

        <!-- Jika ada editing -->
         <?php if (isset($_GET['edit'])): ?>
            <?php $editIndex = $_GET['edit']; ?>
            <form method="POST" action="">
                <input type="hidden" name="index" value="<?php echo $editIndex; ?>">
                
                <input type="password" name="password" value="<?php echo $_SESSION['users'][$editIndex]['password']; ?>" required>
                <button type="submit" name="edit_user">Update User</button>
         </form>
         <?php endif; ?>

         <a href="dashboard.php">Kembali ke dashboard</a>
         </div>
         </body>
         </html>
