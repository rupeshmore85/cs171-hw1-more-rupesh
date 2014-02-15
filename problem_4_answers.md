<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <title>CS171 Homework 1</title>

  <style type="text/css">
 	 	 .col0, .col1, .col2, .col3,  
 	 	 .bar {
				border: 1px solid black;
				padding:2px; 
				border-collapse:collapse;	
 	 		  }
 	     
  </style>
</head>
<body>
  <script src="http://d3js.org/d3.v3.min.js"></script>
  <script>
  var color;
  d3.text("unemp_states_us_nov_2013.tsv", function(error, data){

 	  var parsedTSV = d3.tsv.parseRows(data);
 	  
 	//  var parsedTSVheader = d3.tsv.parseRows(data)[0];
 	
 	  var parsedTSVheader = parsedTSV.shift();
 	  
	  parsedTSVheader.push("Chart");
	  
 	  var sortInd=[0,1,1,1]; // array to hold sort indicator
 	  
 	  d3.select("body").append("h1").text("Unemployment Rates for States");
   	  
      var table = d3.select("body")
      				.append("table")
      				.append("caption")
      				.html("Unemployment Rates for States"+"</br>"+"Monthly Rankings"+"</br>"+"Seasonally Adjusted"+"</br>"+"Dec. 2013p")
      				.attr("class","bar"),
      thead = table.append("thead"),
      tbody = table.append("tbody");
	  
	  var header = thead.selectAll("th")
	  			   .data(parsedTSVheader)
	  			   .enter()
	  			   .append("th")
	  			   .attr("class","bar")
	  			   .text(function(column) {return column; })
	  			   .on("click", function(d,i){	  			   
	  			        sortTable(d,i);
 						applyZebraStyle();
 						 if(i !=0 ){
 	 								if(sortInd[i]==1)
 	 								  {
 	 									 d3.select(this)
                   	 					 .style("cursor", "n-resize");
                   	 				  }
                   	 				else{
                   	 					 d3.select(this)
                   	 					 .style("cursor", "s-resize");
                   	 					}
                   					}
	  			   	})
	  			   .on("mouseover", function(d,i){
 	    				 if(i !=0 ){
 	   								if(sortInd[i]==1)
 	   								  {
 	    								d3.select(this)
                   	 					.style("cursor", "s-resize");
 	   								  }
 	   								 else
 	   								  {
								 	    d3.select(this)
				                    	.style("cursor", "n-resize");
 	   								  }
 	   							    }
 	                });
	 
	 /**
	  * This function applies style to the table
	  */ 			   	
	function applyZebraStyle(){
 				tbody.selectAll("tr")
 	 			.style("background-color", function(d, i) {
 	   														return i % 2 !=0? "orange" : "lightgreen";  
 	 													  });
 							  };  
 							  
 	/**
 	 *This function sorts table in ascending or descending
 	 * order on header click 
 	 */					  
	  function sortTable(d,i){
	  		if(i == 1){
	  			   		if(sortInd[i]==1){
	  			   				sortInd[i]=-1;	// toggle sort Indicator
	  			   				tbody.selectAll("tr").sort(function(a, b) {
	  			   					return d3.ascending(a[i], b[i]);  //sort asc on State
 								});
 							}else{
	  			   				sortInd[i]=1;	//toggle sort Ind
	  			   				tbody.selectAll("tr").sort(function(a, b) {
	  			   					return d3.descending(a[i], b[i]);	//sort desc on State
 								});
 							}
 						}
 					if(i == 2 || i == 3){
	  			   		if(sortInd[i]==1){
	  			   				sortInd[i]=-1;	// toggle sort Indicator
	  			   				tbody.selectAll("tr").sort(function(a, b) {
	  			   				if(a[2] == b[2]){
	  			   					return d3.ascending(a[1], b[1]);  //sort asc on State
	  			   					}
	  			   				else{
	  			   					return d3.ascending(parseFloat(a[2]), parseFloat(b[2]));  //sort asc on Rate
	  			   					}
								});
 							}else{
	  			   				sortInd[i]=1;	//toggle sort Ind
	  			   				tbody.selectAll("tr").sort(function(a, b) {
	  			   				if(a[2] == b[2]){
	  			   					return d3.descending(a[1], b[1]);  //sort desc on State
	  			   					}
	  			   				else{
	  			   					return d3.descending(parseFloat(a[2]), parseFloat(b[2]));  //sort desc on Rate
	  			   					}
 								});
 							}
 						}
	  
	  };
	  
      var rows = tbody.selectAll("tr")
      			 .data(parsedTSV)
                 .enter()
                 .append("tr")
                 .attr("class","bar") 
                 .on('mouseout', function(){
                		 tbody.selectAll("tr")
               			 .style("background-color", function(d, i) {
	  	     			  	return i % 2 ? "orange" : "lightgreen"; 
	  	     		 	  });               
                 })
            	 .on('mouseover', function(){d3.select(this).style("background-color", "yellow")})
	  		     .sort(function(a, b) {
 				                       return d3.ascending(parseFloat(a[2]), parseFloat(b[2]));
										})
			     .style("background-color", function(d, i) {
	  		                                                return i % 2 ? "orange" : "lightgreen";});

      var cells = rows.selectAll("td")
                  .data(function(row) {
                  return d3.range(Object.keys(row).length).map(function(column, i) {
                  return row[Object.keys(row)[i]];
                  });
                  })
      			  .enter()
                  .append("td")
                  .attr("class", function(d,i) {
       			 	if(i == 0)
        			{
        				return "col0";
        			}
        			else if(i == 1)
        			{
        				return "col1";
        			}
        			else if(i == 2)
        			{
        				return "col2";
        			}
        			else if(i == 3)
        			{
        				return "col3";
        			}
        		})                                    
                  .text(function(d) { return d; })                                    
                  rows.insert("td").append("svg")   				 
   				  .attr("width", 30)
   				  .attr("height", 10)
            	  .append("rect")
   				  .attr("height", 10)
   				  .attr("width", function(d) { return parseFloat(d[0]); });
   	 
   	  	   color = d3.scale.linear()
  				  .domain([0, tbody.selectAll("tr")[0].length-1])
  				  .interpolate(d3.interpolateRgb)
  				  .range(["orangered", "silver"]);
  				  
  			/* Color range of Rate column fron Orange to Silver */	  
  				  d3.selectAll(".col2")
 				  .style("background-color", function(d, i) {
 	                                         return color(i);  }); 	                                         
 	               
 	               
 	               
 	               
 	   /*                                  
      		      rows.selectAll(".col0")
 	              .on('mouseover', function(d,i) {
 	              						d3.selectAll(".col0")
 				  						.style("background-color","yellow")
       								})
       			  .on('mouseout', function(d,i){
                		 d3.selectAll(".col0")
               			 .style("background-color", function(d, i) {
	  		   			  	return i % 2 ? "orange" : "lightgreen"; 
	  		   		 	  })         
                   }); 
       */
       			
      });
  </script>
</body>
</html>  				  
