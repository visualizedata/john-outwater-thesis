<template>
    <div>
    <h4 id="dropTitle">Select Category:</h4>
    <div id="drop"></div>
    </div>
</template>


<script>
import * as d3 from 'd3';
export default {
  name: 'bar2',
  data() {
      return{
          berryData:[]
	};
	
  },


  mounted() {

          let _this = this

     d3.csv("./berry.csv").then( function(data, error){
        _this.berryData=data
        _this.drawBars();
     })
      console.log("mounted");
      

  },


 methods: {

drawBars(){
    console.log("drawBars")
    let data=this.berryData
    console.log(this.berryData);

    var margin = {top: 80, right: 180, bottom: 80, left: 180},
        width = 1200 - margin.left - margin.right,
        height = 600 - margin.top - margin.bottom;

    var svg = d3.select(".bar2_graph").append("svg")
        .attr("width", width + margin.left + margin.right)
        .attr("height", height + margin.top + margin.bottom)
        .attr("class", "svgBar")
        .style("position", "relative")
        .style("top", "0px")
        .style("left", "15%")
        .append("g");
        // .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

	var elements = Object.keys(data[0])
		.filter(function(d){
			return (d != "sample_name2");
		});
	var selection = elements[0];

	var y = d3.scaleLinear()
			.domain([d3.min(data, function(d){return +d[selection];})*(0.1), d3.max(data, function(d){return +d[selection];})])
			.range([height, 100]);

	var x = d3.scaleBand()
			.domain(data.map(function(d){ return d.sample_name2;}))
			.range([0, width]);


	var xAxis = d3.axisBottom()
        .scale(x)
        .tickSize(-350)
        .tickSizeOuter(0); 

	var yAxis = d3.axisLeft()
		.scale(y);

	svg.append("g")
    	.attr("class", "x axis")
    	.attr("transform", "translate(0," + height + ")")
    	.call(xAxis)
    	.selectAll("text")
    	.style("font-size", "15px")
        .style("text-anchor", "end")
        .style("font-family", "HelveticaNeue-Light")
        .style("font-weight", "bold")
        .style("fill", "#0b4780")
        .style("letter-spacing", "1.7px")
      	.attr("dx", "-0.2em")
      	.attr("dy", "0.8em")
      	.attr("transform", "rotate(-30)" );


 	svg.append("g")
    	.attr("class", "y axis")
    	.call(yAxis);

	svg.selectAll("circle")
		.data(data)
		.enter()
		.append("circle")
		.attr("class","circle")
		.attr("r", (width/data.length)/6.0)
		.attr("cx", function(d, i){
			return ((width / data.length) * i)+((width / data.length)/2) ;
		})
		.attr("cy", function(d){
			return y(+d[selection]);
		})
		.append("title")
		.text(function(d){
			return d.sample_name2 + " : " + d[selection];
		});

var selector = d3.select("#drop")
    	.append("select")
    	.attr("id","dropdown")
    	.on("change", function(d){
        	selection = document.getElementById("dropdown");

        	y.domain([d3.min(data, function(d){return +d[selection.value];})*(0.1), d3.max(data, function(d){return +d[selection.value];})]);

        	yAxis.scale(y);

            d3.selectAll(".circle")
                .transition()
           		// .duration(2000)
                // .ease("bounce")
	            .attr("cy", function(d){
					return y(+d[selection.value]);
				})
				.attr("cx", function(d, i){
					return ((width / data.length) * i)+((width / data.length)/2) ;
				})
           		.select("title")
           		.text(function(d){
           			return d.sample_name2 + " : " + d[selection.value];
           		});
      
           	d3.selectAll("g.y.axis")
           		.transition()
           		.call(yAxis);

         });

    selector.selectAll("option")
      .data(elements)
      .enter().append("option")
      .attr("text", function(d){
        return d;
      })
      .text(function(d){
        return d;
      })
    //   .style("font-family", "HelveticaNeue-Light")
  }
}

}
</script>

<style>

#drop{
    font-family: "HelveticaNeue-Light";
    background-color: #fffcf6;
    display: flex;
    align-content: left;
    margin-left: 10%;

}

#dropTitle{
    font-family: "HelveticaNeue-Light";
    background-color: #fffcf6;
    display: flex;
    align-content: left;
    margin-left: 10%;
    padding-bottom: 10px;
}

/* #dropdown{
      left: 500px;
} */

.svgBar{
    justify-content: center;
    align-items: center;
    background-color: #fffcf6;
}

.circle {
	fill: #0b4780;
}
.circle:hover {
    stroke: #0b4780;
	fill: white;
}

option{
    font-family:"HelveticaNeue-Light"
}

line {
    fill: lightgray;
}

.axis path{
    opacity: 0;
}
.axis line {
  fill: none;
  stroke: lightgray;
  shape-rendering: crispEdges;
}

</style>
