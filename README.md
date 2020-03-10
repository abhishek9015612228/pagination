# pagination

//procedure
DELIMITER $$
CREATE DEFINER=`root`@`localhost` PROCEDURE `v1_getPagination`(IN `p_limit` INT(11), IN `p_offset` INT(11))
    NO SQL
BEGIN
SELECT First_name,Last_name,Customer_Street FROM customers WHERE 1 ORDER BY First_name ASC LIMIT p_limit,p_offset;
END$$
DELIMITER ;

//end procedure
v1_getPaginationAll 

BEGIN
SELECT First_name,Last_name,Customer_Street FROM customers WHERE 1 ORDER BY First_name ASC LIMIT p_limit,p_offset;
END

getrecords.php
require_once("config.php");
$limit 				= (intval($_GET['limit']) != 0 ) ? $_GET['limit'] : 10;
$offset 			= (intval($_GET['offset']) != 0 ) ? $_GET['offset'] : 0;

//$sql 				= "SELECT countries_name FROM countries WHERE 1 ORDER BY countries_name ASC LIMIT $limit OFFSET $offset";
$sql 				= "call v1_getPagination(?,?)";
try {
  		$stmt 		= $DB->prepare($sql);
		$stmt->bindParam(1, $offset, PDO::PARAM_STR, 4000); 
		$stmt->bindParam(2, $limit, PDO::PARAM_STR, 4000); 
  		$stmt->execute();
  		$results 	= $stmt->fetchAll();
} catch (Exception $ex) {
  echo $ex->getMessage();
}
if (count($results) > 0) {
  foreach ($results as $res) {

    echo '<tr style="height:50px"><td>' . $res['First_name'] . '</td><td>' . $res['Last_name'] . '</td><td>' . $res['Last_name'] . '</td></tr>';
  }
}
