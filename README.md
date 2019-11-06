# Amazon_crawler
#this code will help to crawl every product detail on Amazon
#Just Change the URL to run the script.

<?php




/*

$myfile=fopen("source.html","r");
echo fread($myfile,filesize("source.html"));
flose($myfile);

*/



$file=file_get_contents("source.html");


###name###
  if(preg_match('~id\W+productTitle".*?"\>(.*?)<\/span~is',$file,$match))
   {
     $name = trim($match[1]);
     

     /* preg_replace('/\s+/', ' ', $name); removed all extra spaces or multi lline spaces and 
      	preg_replace('/<.*?>/', '', $name); removed all HTMl tags.*/
     $name = preg_replace('/\s+/', ' ', $name);
     $name = preg_replace('/<.*?>/', '', $name);
	  $arr['name'] = $name;
	  
	    }
#print_r ("Name:-".$name); die;

###Price###
  if(preg_match('~id\W+priceblock_ourprice".*?"\>(.*?)<\/span~is',$file,$match))
   {
     $price = trim($match[1]);
     $price = preg_replace('/\s+/', ' ', $price);
     $price = preg_replace('/<.*?>/', '', $price);
	  $arr['price'] = $price;
	  
   }
#print_r ($arr); die;

###Brand###
  if(preg_match('~id\W+bylineInfo".*?"\>(.*?)<\/a~is',$file,$match))
   {
     $brand = trim($match[1]);
     $brand = preg_replace('/\s+/', ' ', $brand);
     $brand = preg_replace('/<.*?>/', '', $brand);
	  $arr['brand'] = $brand;
   }
//print_r ("Brand:-".$brand); die;



//Review
	if(preg_match('~id\W+acrCustomerReviewText".*?"\>(.*?)<\/span~is',$file,$match))
	{
		$review=trim($match[1]);
		$review = preg_replace('/\s+/', ' ', $review);
        $review = preg_replace('/<.*?>/', '', $review);
		 $arr['review'] = $review;
	}
	//print_r("Review:-".$review);


//Rating
if(preg_match('~id\W+bylineInfo".*?title\=\"(.*?)"\>~is',$file,$match))
   {
		$rating=trim($match[1]);
		$rating = preg_replace('/\s+/', ' ', $rating);
        $rating = preg_replace('/<.*?>/', '', $rating);
		 $arr['rating'] = $rating;
	}
  //print_r("Rating:-".$rating); die;


//Color
if(preg_match('~id\W+variation_color_name".*?selection\"\>(.*?)<\/span~is',$file,$match))
	{
	$color=trim($match[1]);
	$color = preg_replace('/\s+/', ' ', $color);
    $color = preg_replace('/<.*?>/', '', $color);
	 $arr['color'] = $color;
	}
	//print_r("Color:-".$color);
	
	
	
// Bullet Points
if(preg_match('~id\W+replacementPartsFitmentBulletInner".*?script\>(.*?)<\/ul~is',$file,$match))
{
	$bullets=trim($match[1]);
	$bullets = preg_replace('/\s+/', ' ', $bullets);
    $bullets = preg_replace('/<\/\li\>/', '##', $bullets);
    $bullets = preg_replace('/<.*?>/', '', $bullets);


	 $arr['bullets'] = $bullets;
}
//print_r("Description:-<br>".$bullets);


//Product Discription

if(preg_match('~id\W+descriptionAndDetails".*?class\W+disclaim\"\>(.*?)<\/p~is',$file,$match))
	{
	$description=trim($match[1]);
	$description = preg_replace('/\s+/', ' ', $description);
    $description = preg_replace('/<.*?>/', '', $description);
	 $arr['description'] = $description;
	}
	
print_r($arr);

/*
////keys od product Information

 if(preg_match('~class\W+a-row\s*a\-expander\-container\s*a\-expander\-extend\-container\".*?class\W+a\-keyvalue\s*prodDetTable"\s*role\W+presentation"\>(.*?)<\/td~is',$file,$match))
{
	
	$key=trim($match[1]);
	$key = preg_replace('/\s+/', ' ', $key);
	$key = preg_replace('/<.*?>/', '', $key);
	$array['Key']=$key;
	 
	//print_r($array);


}

*/

## SPECIFICATIONS ##

				$list=new DomDocument(); // this linw is used to avoid error => Warning: Creating default object from empty value in C:\xampp\htdocs\new\new.php on line 135
				
				$list->htmlDoc = new DomDocument(); //(but we can avoid waring errors by just typing @ in-front of syntax, @ is used a error control operator in php)
              
               	//$list->testResponse = $file = preg_replace('/\&quot\;/is','"', $file);
                @$list->htmlDoc->loadHTML(html_entity_decode($file, ENT_QUOTES, "UTF-8"));
                $xpath = new DomXpath($list->htmlDoc);
                $spfc_array = array();
               	$nodelist = $xpath->query('//div[@id="prodDetails"]');
                $spec_flag = 0;
                foreach ($nodelist as $n){
                    $spec_flag = 1;
                    $nodelist = $xpath->query('.//table[@id="productDetails_techSpec_section_1"]//tr|//div[@class="a-section table-padding"]', $n);

                    foreach ($nodelist as $c) 
                    	{
                        	$nodelist = $xpath->query('.//th[@class="a-color-secondary a-size-base prodDetSectionEntry"]', $c);
                        	foreach ($nodelist as $z) 
                        	{
                            	$key = trim($z->nodeValue);
                        	}
                       
                        	$nodelist = $xpath->query('.//td[@class="a-size-base"]', $c);
                       	 	foreach ($nodelist as $z) 
                       	 	{
                            $value = trim($z->nodeValue);
                        	}
                        	$spfc_array[$key] = $value;
              				}
               			}
                $product['Product Information']=$spfc_array;


print_r($product);
/*
## SPECIFICATIONS ##
				

              $node=preg_match('~class\W+a-row\s*a\-expander\-container\s*a\-expander\-extend\-container\".*?class\W+a\-keyvalue\s*prodDetTable"\s*role\W+presentation"\>(.*?)<\/table~is',$file);
             
              foreach ($node as $n)
              {

              	$node=$xpath->query(preg_match('~class\W+a\-color\-secondary\s*a\-size-base\s*prodDetSectionEntry"\>"(.*?)<\/tr~is',$node,$match));
              
              	foreach ($node as $key) 
              		{
              			$node=$xpath->query(preg_match('~class\W+a\-color\-secondary\s*a\-size\-base\s*prodDetSectionEntry"\>(.*?)<\/th~is',$z,$match));
              			$node=trim($match[1]);
              			//print_r($node);
              		}
              	}
              	                
//print_r($spfc_array);

*/



 ##Images

  if(preg_match('~id\W+imgTagWrapperId\".*?data\-old\-hires(.*?)onload=\"~is',$file,$match))
{
	$image=trim($match[1]);
	
	
	$image = preg_replace('/\s+/', ' ', $image);
    


	$img['image'] = $image;
	print_r($img);
}
else{
	echo "match not found";
}




##Reviews


##########################
#                        #
#     Customer Name      #
#                        #
##########################
$nameArray['Customer Name'] = NULL;
$url = file_get_contents("https://www.amazon.com/Hunter-53091-Builder-Fan-Brazilian/dp/B00DF4ICX0");
##This Will hit the "See all reviews button in home page and fetch it's url"
if(preg_match('~class\W+a\-link\-emphasis\s*a\-text\-bold"(.*?)See~is',$url,$match))
{
	$review=trim($match[1]);
	$review = preg_replace('/">/', '', $review);
	$review = preg_replace('/href="/', 'https://www.amazon.com', $review);
	$revURL=$review;	
}
//print_r($revURL); die;

$url1=file_get_contents($revURL);

	
			if(preg_match_all('~class\W+a\-profile\-name"\>(.*?)<\/span~is', $url1,$match))
			{
				$result1=$match[1];
				foreach ($result1 as $n) 
				{
					$rename=trim($n);
					$nameArray['Customer Name'] .= $rename . "^^^";
				}
				//print_r($reArray); die;
			}


##PAGE JUMPING


## Next Page URL

			
if(preg_match('~class\W+a\-row\s*a\-spacing\-medium\s*averageStarRatingNumerical"\>(.*?)<\/span~is',$url,$match))
{
	$number=trim($match[1]);
	$number=preg_replace('/[a-z]/','',$number);
	$number=preg_replace('/<.*?>/','',$number);
}
$totalReviews=$number;
$revCount=10;

@$count=$totalReviews/$revCount;
$int_cast = (int)$count;    ##CVount Total number of pages
//echo $int_cast; die;


for($i=0;$i<=$int_cast;$i++)
{
if(preg_match('~class\W+a\-last"\>(.*?)Next~is',$url1,$match))
{
	$nexturl=trim($match[1]);
	$nexturl = preg_replace('/">/','', $nexturl);
	$nexturl = preg_replace('/<a/','', $nexturl);
	$nexturl = preg_replace('/\s+/','',$nexturl);
	$nexturl = preg_replace('/href="/', 'https://www.amazon.com', $nexturl);
	$nexturl = preg_replace('/amp;/','', $nexturl);
	$nextpage=$nexturl;
	
}
//echo $nextpage."<br>";





##user Review
$finalurl=file_get_contents($nextpage);
		if(preg_match_all('~class\W+a\-profile\-name"\>(.*?)<\/span~is', $finalurl,$match))
			{
				$result1=$match[1];
				foreach ($result1 as $n) 
				{
					$rename=trim($n);
					$nameArray['Customer Name'] .= $rename . "^^^^^";
				}
			}
$url1=file_get_contents($nextpage);


}
print_r($nameArray);



######################
#                    #
#     Customer 		 #
#     Rating         #
#                    #
###################### 

$cusReview['Customer Rating'] = NULL;
$url = file_get_contents("https://www.amazon.com/Hunter-53091-Builder-Fan-Brazilian/dp/B00DF4ICX0");
##This Will hit the "See all reviews button in home page and fetch it's url"
if(preg_match('~class\W+a\-link\-emphasis\s*a\-text\-bold"(.*?)See~is',$url,$match))
{
	$review=trim($match[1]);
	$review = preg_replace('/">/', '', $review);
	$review = preg_replace('/href="/', 'https://www.amazon.com', $review);
	$revURL=$review;	
}
//print_r($revURL); die;

$url1=file_get_contents($revURL);
			
			if(preg_match_all('~class\W+a\-icon\-alt"\>(.*?)<\/span~is', $url1,$match))
			{
				$result1=$match[1];
				foreach ($result1 as $n) 
				{
					$rating=trim($n);
					$rating=preg_replace('/out of 5 stars/', '', $rating);
					$cusReview['Customer Rating'] .= $rating . "^^^";
				}
				//print_r($cusReview); die;
			}

##PAGE JUMPING


## Next Page URL

			
if(preg_match('~class\W+a\-row\s*a\-spacing\-medium\s*averageStarRatingNumerical"\>(.*?)<\/span~is',$url,$match))
{
	$number=trim($match[1]);
	$number=preg_replace('/[a-z]/','',$number);
	$number=preg_replace('/<.*?>/','',$number);
}
$totalReviews=$number;
$revCount=10;

@$count=$totalReviews/$revCount;
$int_cast = (int)$count;    ##CVount Total number of pages
//echo $int_cast; die;


for($i=0;$i<=$int_cast;$i++)
{
if(preg_match('~class\W+a\-last"\>(.*?)Next~is',$url1,$match))
{
	$nexturl=trim($match[1]);
	$nexturl = preg_replace('/">/','', $nexturl);
	$nexturl = preg_replace('/<a/','', $nexturl);
	$nexturl = preg_replace('/\s+/','',$nexturl);
	$nexturl = preg_replace('/href="/', 'https://www.amazon.com', $nexturl);
	$nexturl = preg_replace('/amp;/','', $nexturl);
	$nextpage=$nexturl;
	
}
//echo $nextpage."<br>";





##user Review
$finalurl=file_get_contents($nextpage);
			
			if(preg_match_all('~class\W+a\-icon\-alt"\>(.*?)<\/span~is', $finalurl,$match))
			{
				$result1=$match[1];
				foreach ($result1 as $n) 
				{
					$rating=trim($n);
					$rating=preg_replace('/out of 5 stars/', '', $rating);
					$cusReview['Customer Rating'] .= $rating . "^^^^^";
				}
			
			}

$url1=file_get_contents($nextpage);


}
print_r($cusReview);









################################
#                              #
#       Review Date            # 
#                              #
################################


$reDate['Review Date'] = NULL;
$url = file_get_contents("https://www.amazon.com/Hunter-53091-Builder-Fan-Brazilian/dp/B00DF4ICX0");
##This Will hit the "See all reviews button in home page and fetch it's url"
if(preg_match('~class\W+a\-link\-emphasis\s*a\-text\-bold"(.*?)See~is',$url,$match))
{
	$review=trim($match[1]);
	$review = preg_replace('/">/', '', $review);
	$review = preg_replace('/href="/', 'https://www.amazon.com', $review);
	$revURL=$review;	
}
//print_r($revURL); die;

#Date
$url1=file_get_contents($revURL);
			
			if(preg_match_all('~class\W+a\-size\-base\s*a\-color\-secondary\s*review\-date"\>(.*?)<\/span~is', $url1,$match))
			{
				$result1=$match[1];
				foreach ($result1 as $n) 
				{
					$date=trim($n);
					$reDate['Review Date'] .= $date . "^^^";
				}
				//print_r($reDate); die;
			}


##PAGE JUMPING


## Next Page URL

			
if(preg_match('~class\W+a\-row\s*a\-spacing\-medium\s*averageStarRatingNumerical"\>(.*?)<\/span~is',$url,$match))
{
	$number=trim($match[1]);
	$number=preg_replace('/[a-z]/','',$number);
	$number=preg_replace('/<.*?>/','',$number);
}
$totalReviews=$number;
$revCount=10;

@$count=$totalReviews/$revCount;
$int_cast = (int)$count;    ##CVount Total number of pages
//echo $int_cast; die;


for($i=0;$i<=$int_cast;$i++)
{
if(preg_match('~class\W+a\-last"\>(.*?)Next~is',$url1,$match))
{
	$nexturl=trim($match[1]);
	$nexturl = preg_replace('/">/','', $nexturl);
	$nexturl = preg_replace('/<a/','', $nexturl);
	$nexturl = preg_replace('/\s+/','',$nexturl);
	$nexturl = preg_replace('/href="/', 'https://www.amazon.com', $nexturl);
	$nexturl = preg_replace('/amp;/','', $nexturl);
	$nextpage=$nexturl;
	
}
//echo $nextpage."<br>";





##user Review
$finalurl=file_get_contents($nextpage);
if(preg_match_all('~class\W+a\-size\-base\s*a\-color\-secondary\s*review\-date"\>(.*?)<\/span~is', $finalurl,$match))
			{
				$result1=$match[1];
				foreach ($result1 as $n) 
				{
					$date=trim($n);
					$reDate['Review Date'] .= $date . "^^^^^";
				}
			
			}

$url1=file_get_contents($nextpage);


}
print_r($reDate);




###########################
#                         #
#      Purchase           #
#	   Type               #
#                         #
###########################

$reArray1['Purchase Type'] = NULL;	  
$url = file_get_contents("https://www.amazon.com/Hunter-53091-Builder-Fan-Brazilian/dp/B00DF4ICX0");
##This Will hit the "See all reviews button in home page and fetch it's url"
if(preg_match('~class\W+a\-link\-emphasis\s*a\-text\-bold"(.*?)See~is',$url,$match))
{
	$review=trim($match[1]);
	$review = preg_replace('/">/', '', $review);
	$review = preg_replace('/href="/', 'https://www.amazon.com', $review);
	$revURL=$review;	
}
//print_r($revURL); die;

##PAGE JUMPING

$url1=file_get_contents($revURL);
	#Purchase Type
			$purchaseType['Purchase'] = NULL;
			if(preg_match_all('~class\W+a\-size\-mini\s*a\-color\-state\s*a\-text\-bold"\>(.*?)<\/span~is', $url1,$match))
			{
				$result1=$match[1];
				foreach ($result1 as $n) 
				{
					$purchase=trim($n);
					$purchaseType['Purchase'] .= $purchase . "^^^";
				}
			}
		
		//print_r($purchaseType);  die;



## Next Page URL

			
if(preg_match('~class\W+a\-row\s*a\-spacing\-medium\s*averageStarRatingNumerical"\>(.*?)<\/span~is',$url,$match))
{
	$number=trim($match[1]);
	$number=preg_replace('/[a-z]/','',$number);
	$number=preg_replace('/<.*?>/','',$number);
}
$totalReviews=$number;
$revCount=10;

@$count=$totalReviews/$revCount;
$int_cast = (int)$count;    ##CVount Total number of pages
//echo $int_cast; die;


for($i=0;$i<=$int_cast;$i++)
{
if(preg_match('~class\W+a\-last"\>(.*?)Next~is',$url1,$match))
{
	$nexturl=trim($match[1]);
	$nexturl = preg_replace('/">/','', $nexturl);
	$nexturl = preg_replace('/<a/','', $nexturl);
	$nexturl = preg_replace('/\s+/','',$nexturl);
	$nexturl = preg_replace('/href="/', 'https://www.amazon.com', $nexturl);
	$nexturl = preg_replace('/amp;/','', $nexturl);
	$nextpage=$nexturl;
	
}
//echo $nextpage."<br>";

##user Review
$finalurl=file_get_contents($nextpage);
if(preg_match_all('~class\W+a\-size\-mini\s*a\-color\-state\s*a\-text\-bold"\>(.*?)<\/span~is', $finalurl,$match))
			{
				$result1=$match[1];
				foreach ($result1 as $n) 
				{
					$purchase=trim($n);
					$purchaseType['Purchase'] .= $purchase . "^^^^^";
				}
			}

$url1=file_get_contents($nextpage);


}
print_r($purchaseType);

#######################################
#   								                  #
#     Customer Detailed Review        #
#                                     #
#                                     #
#######################################

$reArray1['Customer Review'] = NULL;	  
$url = file_get_contents("https://www.amazon.com/Hunter-53091-Builder-Fan-Brazilian/dp/B00DF4ICX0");

if(preg_match('~class\W+a\-link\-emphasis\s*a\-text\-bold"(.*?)See~is',$url,$match))
{
	$review=trim($match[1]);
	$review = preg_replace('/">/', '', $review);
	$review = preg_replace('/href="/', 'https://www.amazon.com', $review);
	$revURL=$review;	
}
//print_r($revURL); die;

##PAGE JUMPING

$url1=file_get_contents($revURL);
if(preg_match_all('~class="a-size-base review-text review-text-content">(.*?)<\/span~is',$url1,$match))
{
	$result=$match[1];
	$reArray['Customer Review'] = NULL;
	foreach ($result as $n) 
	{
		$data=trim($n);
		$data=preg_replace('/<.*?>/', '', $data);
		$reArray['Customer Review'] .= $data . "^^^";
	}
	
}
//print_r($reArray);

## Next Page URL
if(preg_match('~class\W+a\-row\s*a\-spacing\-medium\s*averageStarRatingNumerical"\>(.*?)<\/span~is',$url,$match))
{
	$number=trim($match[1]);
	$number=preg_replace('/[a-z]/','',$number);
	$number=preg_replace('/<.*?>/','',$number);
}
$totalReviews=$number;
$revCount=10;

@$count=$totalReviews/$revCount;
$int_cast = (int)$count;
//echo $int_cast; die;


for($i=0;$i<=$int_cast;$i++)
{
if(preg_match('~class\W+a\-last"\>(.*?)Next~is',$url1,$match))
{
	$nexturl=trim($match[1]);
	$nexturl = preg_replace('/">/','', $nexturl);
	$nexturl = preg_replace('/<a/','', $nexturl);
	$nexturl = preg_replace('/\s+/','',$nexturl);
	$nexturl = preg_replace('/href="/', 'https://www.amazon.com', $nexturl);
	$nexturl = preg_replace('/amp;/','', $nexturl);
	$nextpage=$nexturl;
	
}
//echo $nextpage."<br>";
##user Review
$finalurl=file_get_contents($nextpage);
if(preg_match_all('~class\W+a\-size\-base\s*review\-text\s*review\-text\-content"\>(.*?)<\/span~is',$finalurl,$match))
{
	$result=$match[1];
	
	foreach ($result as $n) 
	{
		$data=trim($n);
		$data=preg_replace('/<.*?>/', '', $data);
		$reArray1['Customer Review'] .= $data . "^^^^^";
	}
}

$url1=file_get_contents($nextpage);

}
print_r($reArray1);

?>

