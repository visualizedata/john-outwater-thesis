<template>
    <div id="radial">
        <div :id="'radial-'+id">
            <svg>
            </svg>
        </div>
    </div>
</template>

<script>
// import radialData from "~/assets/radialBerry.json"
import * as d3 from 'd3';
export default {
  name: 'radial',
  data() {
      return{
    width: 600, 
    // width: Math.min(700, window.innerWidth - 10) - margin.left - margin.right,
    height: 600,
    // height: Math.min(width, window.innerHeight - margin.top - margin.bottom - 20),
	margin: {top: 25, left: 25, bottom: 25, right: 25 }
    };
  },
 props: [
	 "allData",
	 "id"
 ],
 methods: {
    RadarChart(id, data, options) {

	var cfg = {
	 width: 250,				//Width of the circle
	 height: 250,				//Height of the circle
	 margin: {top: 100, right: 100, bottom: 100, left: 100}, //The margins of the SVG
	 levels: 5,				//How many levels or inner circles should there be drawn
	 maxValue: 1.0, 		//What is the value that the biggest circle will represent
	 labelFactor: 1.25, 	//How much farther than the radius of the outer circle should the labels be placed
	 wrapWidth: 60, 		//The number of pixels after which a label needs to be given a new line
	 opacityArea: 0.35, 	//The opacity of the area of the blob
	 dotRadius: 4, 			//The size of the colored circles of each blob
	 opacityCircles: 0.1, 	//The opacity of the circles of each blob
	 strokeWidth: 1, 		//The width of the stroke around each blob
	 roundStrokes: true,	//If true the area and stroke will follow a round path (cardinal-closed)
	 color: d3.schemeCategory10	//Color function
	};
	
	//Put all of the options into a variable called cfg
	if('undefined' !== typeof options){
	  for(var i in options){
		if('undefined' !== typeof options[i]){ cfg[i] = options[i]; }
	  }//for i
	}//if
	
	if(this.id != null){data=[data[this.id]]}
	//Create an array of extent values for each category in the data
	//Will be used for normalization and creating unique scales on each axis
	  let extent_Value = [];
	  for(var i=0; i<this.allData[0].length; i++){
	    extent_Value.push(d3.extent(this.allData.map(d => d[i].value)));
	  };
	  console.log(extent_Value)

	//If the supplied maxValue is smaller than the actual one, replace by the max in the data
	var maxValue = Math.max(cfg.maxValue, d3.max(data, function(i){return d3.max(i.map(function(o){return o.value;}))}));
		
	var allAxis = (data[0].map(function(i, j){return i.axis})),	//Names of each axis
		// allValue = (data[0].map(function(n, t){return n.value})),
		total = allAxis.length,					//The number of different axes
		radius = Math.min(cfg.width/2, cfg.height/2), 	//Radius of the outermost circle
		Format = d3.format('%'),			 	//Percentage formatting
		angleSlice = Math.PI * 2 / total;		//The width in radians of each "slice"
		
		console.log(radius);
	
	//Scale for the radius
	var rScale = d3.scaleLinear()
		.domain([0, maxValue])
		.range([0, radius]);
		
	var rScale2 = extent_Value.map(el => d3.scaleLinear()
					.domain(el)
					.range([70, radius]))
	
	//Create the container SVG and g//
	//Remove whatever chart with the same id/class was present before
	d3.select('#radial-'+this.id).select("svg").remove();
	
	//Initiate the radar chart SVG
	var svg = d3.select('#radial-'+this.id).append("svg")
			.attr("width",  cfg.width + cfg.margin.left + cfg.margin.right)
			.attr("height", cfg.height + cfg.margin.top + cfg.margin.bottom)
			.attr("class", "radar"+id);
	//Append a g element		
	var g = svg.append("g")
			.attr("transform", "translate(" + (cfg.width/2 + cfg.margin.left) + "," + (cfg.height/2 + cfg.margin.top) + ")");
	
	//Filter for the outside glow
	var filter = g.append('defs').append('filter').attr('id','glow'),
		feGaussianBlur = filter.append('feGaussianBlur').attr('stdDeviation','2.5').attr('result','coloredBlur'),
		feMerge = filter.append('feMerge'),
		feMergeNode_1 = feMerge.append('feMergeNode').attr('in','coloredBlur'),
		feMergeNode_2 = feMerge.append('feMergeNode').attr('in','SourceGraphic');

	//Draw the Circular grid//
	//Wrapper for the grid & axes
	var axisGrid = g.append("g").attr("class", "axisWrapper");
	
	//Draw the background circles
	axisGrid.selectAll(".levels")
	   .data(d3.range(1,(cfg.levels+1)).reverse())
	   .enter()
		.append("circle")
		.attr("class", "gridCircle")
		.attr("r", function(d, i){return radius/cfg.levels*d;})
		.style("fill", "white")
		.style("stroke", "#CDCDCD")
		.style("fill-opacity", cfg.opacityCircles)
		.style("filter" , "url(#glow)");

	//Text indicating at what % each level is
	// axisGrid.selectAll(".axisLabel")
	//   .data(d3.range(1,(cfg.levels+1)).reverse())
	//   .enter().append("text")
	//   .attr("class", "axisLabel")
	//   .attr("x", 4)
	//   .attr("y", function(d){return -d*radius/cfg.levels;})
	//   .attr("dy", "0.4em")
	//   .style("font-size", "10px")
	//   .attr("fill", "#737373")
	//   .text(function(d,i) { return Format(maxValue * d/cfg.levels); });

	//Draw the axes//
	//Create the straight lines radiating outward from the center
	var axis = axisGrid.selectAll(".axis")
		.data(allAxis)
		.enter()
		.append("g")
		.attr("class", "axis");
		
	//Append the lines
	axis.append("line")
		.attr("x1", 0)
		.attr("y1", 0)
		.attr("x2", function(d, i){ return rScale(maxValue*1.0) * Math.cos(angleSlice*i - Math.PI/2); })
		.attr("y2", function(d, i){ return rScale(maxValue*1.0) * Math.sin(angleSlice*i - Math.PI/2); })
		.attr("class", "line")
		.style("stroke", "lightgray")
		.style("stroke-width", "1px");

	//Append the labels at each axis
	axis.append("text")
		.attr("class", "legend")
		.style("font-size", "5px")
		.attr("text-anchor", "middle")
		.attr("dy", "0.35em")
		.attr("x", function(d, i){ return rScale(maxValue * cfg.labelFactor) * Math.cos(angleSlice*i - Math.PI/2); })
		.attr("y", function(d, i){ return rScale(maxValue * cfg.labelFactor) * Math.sin(angleSlice*i - Math.PI/2); })
		.text(function(d){return d})
		.call(wrap, cfg.wrapWidth);

	//Draw the radar chart blobs//
	//The radial line function
	var radarLine = d3.lineRadial()
		// .interpolate("linear-closed")
		.radius(function(d,i) { return rScale2[i](d.value); })
		.angle(function(d,i) {	return i*angleSlice; })
		.curve(d3.curveCardinalClosed);
		
	// if(cfg.roundStrokes) {
	// 	radarLine.interpolate("cardinal-closed");
	// }
				
	//Create a wrapper for the blobs	
	var blobWrapper = g.selectAll(".radarWrapper")
		.data(data)
		.enter().append("g")
		.attr("class", "radarWrapper");
			
	//Append the backgrounds	
	blobWrapper
		.append("path")
		.attr("class", "radarArea")
		.attr("d", function(d,i) { return radarLine(d); })
		.style("fill", "0b4780")
		.style("fill-opacity", cfg.opacityArea)
		// .on('mouseover', function (d,i){
		// 	//Dim all blobs
		// 	d3.selectAll(".radarArea")
		// 		.transition().duration(200)
		// 		.style("fill-opacity", 0.1); 
		// 	//Bring back the hovered over blob
		// 	d3.select(this)
		// 		.transition().duration(200)
		// 		.style("fill-opacity", 0.7);	
		// })
		// .on('mouseout', function(){
		// 	//Bring back all blobs
		// 	d3.selectAll(".radarArea")
		// 		.transition().duration(200)
		// 		.style("fill-opacity", cfg.opacityArea);
		// });
		
	//Create the outlines	
	blobWrapper.append("path")
		.attr("class", "radarStroke")
		.attr("d", function(d,i) { return radarLine(d); })
		.style("stroke-width", cfg.strokeWidth + "px")
		.style("stroke", function(d,i) { return cfg.color(i); })
		.style("fill", "none")
		// .style("filter" , "url(#glow)");
	
	// Append the circles
	// blobWrapper.selectAll(".radarCircle")
	// 	.data(function(d,i) { return d; })
	// 	.enter().append("circle")
	// 	.attr("class", "radarCircle")
	// 	.attr("r", cfg.dotRadius)
	// 	.attr("cx", function(d,i){ return rScale2[i](d.value) * Math.cos(angleSlice*i - Math.PI/2); })
	// 	.attr("cy", function(d,i){ return rScale2[i](d.value) * Math.sin(angleSlice*i - Math.PI/2); })
	// 	.style("fill", function(d,i,j) { return cfg.color(j); })
	// 	.style("fill-opacity", 0.8);


	//Append invisible circles for tooltip//
	//Wrapper for the invisible circles on top
	var blobCircleWrapper = g.selectAll(".radarCircleWrapper")
		.data(data)
		.enter().append("g")
		.attr("class", "radarCircleWrapper");
		
	//Append a set of invisible circles on top for the mouseover pop-up
	blobCircleWrapper.selectAll(".radarInvisibleCircle")
		.data(function(d,i) { return d; })
		.enter().append("circle")
		.attr("class", "radarInvisibleCircle")
		.attr("r", cfg.dotRadius*1.5)
		.attr("cx", function(d,i){ return rScale(d.value) * Math.cos(angleSlice*i - Math.PI/2); })
		.attr("cy", function(d,i){ return rScale(d.value) * Math.sin(angleSlice*i - Math.PI/2); })
		.style("fill", "none")
		.style("pointer-events", "all")
		.on("mouseover", function(d,i) {
			newX =  parseFloat(d3.select(this).attr('cx')) - 10;
			newY =  parseFloat(d3.select(this).attr('cy')) - 10;
					
			tooltip
				.attr('x', newX)
				.attr('y', newY)
				.text(Format(d.value))
				.transition().duration(200)
				.style('opacity', 1);
		})
		.on("mouseout", function(){
			tooltip.transition().duration(200)
				.style("opacity", 0);
		});
		
	//Set up the small tooltip for when you hover over a circle
	var tooltip = g.append("text")
		.attr("class", "tooltip")
		.style("opacity", 0);
	
	

	//HELPER FUNCTION//
	//Taken from http://bl.ocks.org/mbostock/7555321
	//Wraps SVG text	
	function wrap(text, width) {
	  text.each(function() {
		var text = d3.select(this),
			words = text.text().split(/\s+/).reverse(),
			word,
			line = [],
			lineNumber = 0,
			lineHeight = 1.4, // ems
			y = text.attr("y"),
			x = text.attr("x"),
			dy = parseFloat(text.attr("dy")),
			tspan = text.text(null).append("tspan").attr("x", x).attr("y", y).attr("dy", dy + "em");
			
		while (word = words.pop()) {
		  line.push(word);
		  tspan.text(line.join(" "));
		  if (tspan.node().getComputedTextLength() > width) {
			line.pop();
			tspan.text(line.join(" "));
			line = [word];
			tspan = text.append("tspan").attr("x", x).attr("y", y).attr("dy", ++lineNumber * lineHeight + dy + "em").text(word);
		  }
		}
	  });
	}//wrap
}//RadarChart
 },
  computed: {},
  mounted() {

            // const data = radialData;

      		var color = d3.scaleOrdinal()
			    .range(["#0b4780"]);
				
			var radarChartOptions = {
			//   w: width,
			//   h: height,
			//   margin: margin,
			  maxValue: 1.0,
			  levels: 5,
			  roundStrokes: true,
			  color: color
			};
			//Call function to draw the Radar chart
			this.RadarChart(`.radarChart`, this.allData, radarChartOptions)
			// console.log(radialData);
  }
}
</script>

<style scoped>
/* svg text{
	font-size:5px
} */
</style>