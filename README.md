<?php
$file = __DIR__ . "/sisendid.txt";
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $input = $_POST['sisend'] ?? '';
    file_put_contents($file, $input . "\n", FILE_APPEND);
}
?>
<form method="POST">
  <input name="sisend" placeholder="Sisesta midagi...">
  <button type="submit">Saada</button>
</form>
