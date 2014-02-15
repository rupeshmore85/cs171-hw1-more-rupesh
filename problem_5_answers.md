<!DOCTYPE html>
<html>
<head>
<script src="http://d3js.org/d3.v3.min.js"></script>
  <style type="text/css">
    rect {
       
        fill-opacity: .8;
    }
    text {
        font: 10px sans-serif;
    }
    text.barText{
    	 font: 10px sans-serif;
    }
     text.stateText{
    	 font: 10px sans-serif;
    }
  </style>
</head>
<body>
<br>
<br>
<form>
<label><input class="rdState" type="radio" name="order" value="state"> State</label>
<label><input class="rdRate" type="radio" name="order" value="rate" checked> Rate</label>
</form>

  <script type="text/javascript">
var data1;
    var margin = {top: 50, bottom: 10, left:300, right: 40};
    var width = 900 - margin.left - margin.right;
    var height = 900 - margin.top - margin.bottom;

    var xScale = d3.scale.linear().range([0, width]);
    var yScale = d3.scale.ordinal().rangeRoundBands([0, height], .8, 0);

    var bar_height = 15;

	var rateSortFlag =1;
	var stateSortFlag =1;
	
    var state = function(d) { return d.State; };

    var svg = d3.select("body").append("svg")
      .attr("width", width+margin.left+margin.right)
      .attr("height", height+margin.top+margin.bottom); 


    var g = svg.append("g")
      .attr("transform", "translate("+margin.left+","+margin.top+")");

/**
 *This function sorts the dataset in ascending
 * order based on Rate
 */
function sortAscendingRate(data){
      	var j,k;
		var temp;
	for( j=0; j<data.length; j++)
	{
		for(k=0; k<data.length; k++)
		{
			if(parseFloat(data[j].Rate)<parseFloat(data[k].Rate))
			{
				temp = data[j];
				data[j]= data[k];
				data[k]= temp;
			}
		}	

	}
}

/**
 *This function sorts the dataset in descending
 * order based on Rate
 */
function sortDescendingRate(data){
      	var j,k;
		var temp;
	for( j=0; j<data.length; j++)
	{
		for(k=0; k<data.length; k++)
		{
			if(parseFloat(data[j].Rate)>parseFloat(data[k].Rate))
			{
				temp = data[j];
				data[j]= data[k];
				data[k]= temp;
			}
		}	

	}
}

/**
 *This function sorts the dataset in ascending
 * order based on State
 */
function sortAscendingState(data){
      	var j,k;
		var temp;
	for( j=0; j<data.length; j++)
	{
		for(k=0; k<data.length; k++)
		{
			if(data[j].State<data[k].State)
			{
				temp = data[j];
				data[j]= data[k];
				data[k]= temp;
			}
		}	

	}
}

/**
 *This function sorts the dataset in descending
 * order based on State
 */
function sortDescendingState(data){
      	var j,k;
		var temp;
	for( j=0; j<data.length; j++)
	{
		for(k=0; k<data.length; k++)
		{
			if(data[j].State>data[k].State)
			{
				temp = data[j];
				data[j]= data[k];
				data[k]= temp;
			}
		}	

	}
}

/*
 * This function displays the chart
 */
d3.tsv("unemp_states_us_nov_2013.tsv", function(data) {
      var max = d3.max(data, function(d) { return d.Rate; } );
      var min = 0;

      xScale.domain([min, max]);
      yScale.domain(data.map(state));

	//display title
	g.append("text") 
		.attr("x", (width / 2)) 
      	.attr("y", 0 - (margin.top / 2))
      	.attr("text-anchor", "middle") 
      	.style("font-size", "16px") 
      	.style("text-decoration", "underline") 
      	.text("Unemployment Rates for States")
      		.attr("fill","black");
      var  color = new Array();		
      var groups = g.append("g")
        .selectAll("g")
        .data(data)
      .enter()
        .append("g")
        .attr("transform", function(d, i) { return "translate(0, " + yScale(d.State) +")"; });
      		
      //display bar chart
      var bars = groups
        .append("rect")
        .attr("width", function(d) { return xScale(d.Rate); })
        .attr("height", bar_height)
         
   		// define color scale     	      	
  		 color = d3.scale.linear()
  				  .domain([0, svg.selectAll("rect")[0].length])
  				  .interpolate(d3.interpolateRgb)
  				  .range(["orangered", "silver"]);
  		
  		//apply color scale to chart
  		svg.selectAll("rect")		  
  		.attr("fill",function(d,i){
        	 return color(d.Rate);
        	}) 
        	
        // display Rate at the end of the bar chart 				        	
        groups.append("text")
		 .attr("class","barText") 
        .attr("x", function(d) { return xScale(d.Rate); })
        .attr("y", function(d) { return bar_height/2; })
        .attr("text-anchor","middle")
         .attr("dx", -10)
        .attr("dy", bar_height/3)
        .text(function(d) { return d.Rate; })
       	.attr("fill","white");
       	
       	//display State on Y axis
       	groups
       	.append("text")
       	.attr("class","stateText") 
       	.attr("text-anchor","end")
        .attr("x", function(d) { return -2; })
        .attr("y", function(d) { return (bar_height-3); })
        .text(function(d) { return d.State; })
       	.attr("fill","teal")
       	
       	//bind sort functions to radio buttons
       	d3.selectAll("input.rdState").on("click", reorderState);
       	d3.selectAll("input.rdRate").on("click", reorderRate);
     
     	/*
     	* This function transforms the chart 
     	*/
      	function transformChart(){
      	
      		//update bar chart
       		svg.selectAll("rect")
       		.data(data)
       		.transition()
  			.duration(100)
  			.attr("width", function(d) { return xScale(d.Rate); })
  			.attr("x", (width / 150))
  			.attr("fill",function(d,i){
        	 return color(d.Rate);
        	})
       		
       		//update Rate on the bar chart
       		 svg.selectAll("text.barText")
       		 .data(data)
       		.transition()
  			.duration(100)
        	.attr("x", function(d) { return xScale(d.Rate); })
        	.attr("text-anchor","middle")
        	 .attr("dx", -10)
        	.attr("dy", bar_height/3)
        	.text(function(d) { return d.Rate; })
        
       		 //Update States
        	 svg.selectAll("text.stateText")
       	 	.data(data)
       		.transition()
  			.duration(100)
       		 .attr("x", function(d) { return -2; })
        	.attr("text-anchor","end")
        	.attr("dx", -10)
        	.attr("dy", bar_height/4)
        	.text(function(d) { return d.State; })
        
        
       	}
          	
        /*
        *This function calculates 
        * the scale
        */
        function calculateScale()
        {

     	 var max = d3.max(data, function(d) { return d.Rate; } );
    	  var min = 0;

     	 xScale.domain([min, max]);
     	 yScale.domain(data.map(state));
        
        }
        
        //This function sorts chart as per state
        function reorderState(){
        
       		if(stateSortFlag ==1 ){
       			sortAscendingState(data);
       			stateSortFlag=-1;
       		}else{
       			sortDescendingState(data);
       			stateSortFlag=1;
       		}
      		calculateScale();
       		transformChart();
        }
        
        //This function sorts chart as per Rate
       	function reorderRate()
       	{
       		if(rateSortFlag ==1 ){
				sortAscendingRate(data);
       			rateSortFlag=-1;
       		}else{
       			sortDescendingRate(data);
       			rateSortFlag=1;
       		}
      		calculateScale();
       		transformChart();
       	 } 
	 });
    

  </script>
</body>
</html>