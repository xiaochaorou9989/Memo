# Upload 相关 <PHP>

### $_FILES

表单 `enctype` 属性必须设置为 `multipart/form-data`。

如果是多文件，除设置表单字段 `multiple` 属性外，字段名(field) 必须为 `字段名 + []` 格式，即 `field_name[]`。否则 `$_FILES` 将只会获得第一个文件。

附:

多文件时 `$_FILES` 获取到的数据格式如下：

```
array (size=2)
  'your_file_field_name' => 
    array (size=5)
      'name' => 
        array (size=3)
          0 => string 'unsplash_201901251706092004.jpg' (length=31)
          1 => string 'unsplash_201901251711282037.jpg' (length=31)
          2 => string 'unsplash_201901251714249958.jpg' (length=31)
      'type' => 
        array (size=3)
          0 => string 'image/jpeg' (length=10)
          1 => string 'image/jpeg' (length=10)
          2 => string 'image/jpeg' (length=10)
      'tmp_name' => 
        array (size=3)
          0 => string '/local/path/to/temp/php555F.tmp' (length=31)
          1 => string '/local/path/to/temp/php562B.tmp' (length=31)
          2 => string '/local/path/to/temp/php564B.tmp' (length=31)
      'error' => 
        array (size=3)
          0 => int 0
          1 => int 0
          2 => int 0
      'size' => 
        array (size=3)
          0 => int 13712208
          1 => int 8696802
          2 => int 6275104
```

**注意**：只要字段名以多文件的格式定义了，即便是实际只上传了一个文件，`$_FILES` 获取到的数据格式依然如上所示。

### php://input

该方式获取数据时表单 `enctype` 属性必须为 `application/x-www-form-urlencoded`，否则将获取不到数据。

`php://input` 可以直接获取 POST 过来的文件二进制流，然后根据二进制的文件头判断文件类型，最后将二进制流写入服务器存储文件即可。

通过文件头判断文件类型的方法片段：

```
function getFileType($stream) {
	$bin = mb_substr($stream, 0, 2);
	$str_info  = @unpack("C2chars", $bin);
	$type_code = intval($str_info['chars1'].$str_info['chars2']);
	$file_type = '';
	switch ($type_code) {
		case 7790:
			$file_type = 'exe';
			break;
		case 7784:
			$file_type = 'midi';
			break;
		case 8075:
			$file_type = 'zip';
			break;
		case 8297:
			$file_type = 'rar';
			break;
		case 255216:
			$file_type = 'jpg';
			break;
		case 7173:
			$file_type = 'gif';
			break;
		case 6677:
			$file_type = 'bmp';
			break;
		case 13780:
			$file_type = 'png';
			break;
		default:
			$file_type = 'unknown';
			break;
	}
	return $file_type;
}
```
