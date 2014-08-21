# What's up with jank?

* Jank ruins the illusion of momvement
* User exerience is reduced
* Consistency is better than speed
* Facebook A/B test reducued scrolling fps to 30 and KPI for engagement collapsed



# Warm up rounds

# Document loaded (first img)
* DOM + CSSOM = Render Tree
* Render tree laid out and painted
* Composited (Pasted) onto the screen for your pleasure



# Kitty

* Done with $animate
* Updating the DOM with every frame
* Dev tools
	* Paint rect
	* Mobile Benchmark
* Big paint overhead - gets bigger with more nodes
* Massive JS overhead



# Document changed (2nd img)

* When something changes it's appearance, we have to repaint
* The bigger the repaint area, the more work it takes
* We have to get it all calculated, rendered and out the door in 16 ms to hit 60 FPS



#Cage

* Using animate so NO js overhead
* Smaller dirty area, no paint union
* We're compositing, yay


# Layer promotion (Third img)

* The browser noticed we were animating so it upgraded the nodes to composite layers 
* This sandboxes each cage so they dont dirty the rest of the document
* But we're still painting!!! Why?



# Properties that trigger layout

* WTF we're not doing any of that stuff
* Oh, we'rte changing the size of the container so we're implicitly changing the background size.
* Let's fix that


# Grizzley

* Lets move all the resizing onto the gpu
* Dev tools
	* Paint rect
	* Mobile Benchmark
* Takeaways:
	* No Painting
	* No scripting
	* Awesome



# Layout Thrashing

* Thrashing happens when JS get's itself tangled up in a read write cycle where it can't optimise
* A layout is when the browser consults the rendertree to set the geometry of a node
* Reading certain properties also cause the browser to recalculate tehem even if they are the same
* A single layout is not as expensive as a paint - but they tend to waterfall
* Changes in geometry, as we have seen with Kitty, can also cause repaints
* Browsers batch their layout writes when they are confident pnothing has changed



# Layout Thrashing EX 1
* Move the read out of the loop, now the browser can cache the writes



# Layout Thrashing EX 2
* In an ideal world, you'd do all your reads first thing.
* Requestanimationframe can help in situations where you can't get all your read in one place

#Paint storms
* They are terrible
* Caused by something causing a paint on every frame