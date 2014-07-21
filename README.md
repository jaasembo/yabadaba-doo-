<?php
//here is a little pagination script
//Load my dependancies first I only need one but for clarity ill have them both
require_once "../moss/Database_Connect.php";
require_once "../moss/app_config.php";
$per_page = 6;


$pages_query = mysql_query("SELECT COUNT('id') FROM cars");
$pages=ceil(mysql_result($pages_query, 0) / $per_page);

$page = (isset($_GET['page'])) ? (int)$_GET['page'] : 1;
$start = ($page-1) * $per_page;
$query = "SELECT*FROM cars ORDER BY YEAR DESC LIMIT $start,$per_page";
 $result=mysql_query($query)
or die(mysql_error());

//CREATE TABLE HEADERS
echo"<table id='Grid' style='width:80%'><tr>";
echo"<th style='width:50px'>Make</th>";
echo"<th style='width:50px'>Model</th>";
echo"<th style='width:50px'>Asking Price</th>";

echo"</tr>\n";

$class = "odd";

while($result_ar=mysql_fetch_assoc($result)){
echo"<tr class=\"$class\">";
echo"<td><a style='color:white;' href='viewcar.php?VIN=".$result_ar['VIN']."'>" . $result_ar['Make']. "<a/></td>";
echo"<td>".$result_ar['Model']."</td>"; 
echo"<td>".$result_ar['ASKING_PRICE']."</td>";
echo"</td></tr>\n";


if($class=="odd"){
     $class="even";
}else{
$class="odd";
}
}
echo"</table>";
echo '<div class="my_class">';
$prev =$page-1;
$next=$page+1;
if(!($page<=1)){
echo"<a  href='Show_Inventory.php?page=$prev'>prev<a/>\n";
}
if($pages >=1){
for($x=1;$x<=$pages;$x++){
echo ($x==$page) ?  '<b><a style="color:red;" href="?page='.$x.'">'.$x.'</a></b> ':'<a href="?page='.$x.'">'.$x.'</a> ';
}
if(!($page>=$pages)){
echo"<a  href='Show_Inventory.php?page=$next'>next<a/>";
}
}
echo '</div>';
?>


