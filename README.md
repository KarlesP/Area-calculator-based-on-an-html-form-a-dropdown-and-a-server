# Webapp calculator
Calculating with SQL arrays


<!DOCTYPE html>
<html lang="en">
<head>
  <meta http-equiv="content-type" content="text/html; charset=UTF-8">
      <title>test calculation page.</title>
    </head>
    <body>
    <?php

//creating a function to connect to the database
    function getRowByQuery($query){
   //setting up a host
      @$host = '';
  //setting up the user
      @$user = '';
  //setting up the password
      @$pass = '';
  //setting up the database
      $db_name= '';
  //opening the connection
      $conn = new mysqli($host, $user, $pass, $db_name);
  //returning the connection as a variable so every time we call the function we return the array
      $returnQuery = mysqli_query($conn,$query);
      return $returnQuery;

    };

//creating a function for our query
    function getQueryTitle(){
      return " SELECT nid, title, field_paint_times_value, field_paint_efficeincy__value".
            " FROM node n INNER JOIN ".
            " field_data_field_paint_times t on t.entity_id = nid INNER JOIN ".
            " field_data_field_paint_efficeincy_ e on e.entity_id = nid ".
            " group by n.nid; ";
    }


    //a funtion that creates a drop down list
    function generateColorList(){
      $selectHeaderStr = "<select class='colorlist-items colorlist' name='title'  id='colorlist' width='250px'>";
      $selectHeaderEnd = "</select>";
      $qryTitle = getQueryTitle();
      $connQuery = getRowByQuery($qryTitle);

      echo $selectHeaderStr;
      while($row = mysqli_fetch_array($connQuery)){
          echo "<option  id='" . $row['title'] ."-" . $row['nid'] ."' class='colorlist-item Colorlist' data-times='" . $row['field_paint_times_value'] ."'  data-efficiency='" . $row['field_paint_efficeincy__value'] ."' value='" . $row['title'] ."'>". $row['title'] . "</option>";
          $p = $row[2];echo $p;echo "</br>";
          $q = $row[3];echo $q ;echo "</br>";
      }
      echo $selectHeaderEnd;echo '</br>';
    }
  echo generateColorList();


  ?>





  <form action="" method="POST" id="myForm">
 
    <br />Number of doors:
    <input type="number" min="0" max="9999999999999" id="Nd" name="D" value="0"/>
    
    <input type="button" value="Υπολογισμός Εμβαδού και Ποσοστού απόδοσης μπογιάς" name="sub" id="subm" onClick="area()">
  </form>
  <script src="jquery-3.1.0.min.js"></script>
  <script type="text/javascript">

//creating the function so we can calculate the area
  
  function area() {
 
 //creating a variable where we store the input value from the form
  var doors = parseInt(document.getElementById("Nd").value,10);
 //changing the previous variable based on a parameter (parameter=5.0212)
  var doors_w_par = doors * 5.0212;

//calculating the area of our doors 
  var area_a = doors_w_par;
//returning the last calculation
  return area_a;
}
//and now for out jquery function

//so we will need to load the function everytime we want to recalculate
window.onload=function() {
//inputing the values from the html element with id "subm"
  document.getElementById("subm").onclick=function() {
  //printing area
    console.log(area());
    //taking the function that we created and we ready it so we can print our caclculations 
    $(document).ready(function(){
    //selecting the right value
    $('#colorlist').on("click",function() {
    //creatin null values so we can calculate with every "click"
        var str= "";
        var eff = "";
        var time = "";
        //selecting a value from our drop-down menu
        $( "select option:selected" ).each(function() {
        //inputing the values that our selection bring inside 3 diffrent variables
          time += $( this ).attr('data-times');
          eff += $( this ).attr('data-efficiency');
          str += $( this ).text() + " ";
       //setting up an if statement and outputing the right answers
          if (area() > 0){
          $('#area').text("For "+area()+" you will need "+area()*eff+" liter of paint." )
          $('#total').text("Our caclculations shows that the times that you have to repaint your wall are :"+time)}
          else {$('#area').text("hmm,there must be a mistake, try again with another value.")}
        });


      })});

  }
}



  </script>
  <br />
  //bringing the outputs from our javascript function into the user page 
  <div id="area"></div><br />
  <div id="total"></div><br />
<p id="times"></p>
<p id="efficiency"></p>
</body>
</html>
