---
title: Optical Flow
keywords:
order: 0
---

- [7.1 Optical Flow](#7.1-optical-flow)
- [7.2 Lukas-Kanade Method](#7.2-lukas-kanade method)
- [7.3 Pyramids for Large Motion](#7.3-pyramids-for-large-motion)
	- [7.3.1 Previous Assumptions and Large Motion](#7.3.1-previous-assumptions-and-large-motion) 
	- [7.3.2 Optical Flow Estimation with Pyramids](#7.3.2-optical-flow-estimation-with-pyramids) 
	- [7.3.3 Optical Flow Results](#7.3.3-optical-flow-results) 
- [7.4 Horn-Schunk Method](#7.4-horn-schunk-method)
- [7.5 Applications](#7.5-applications)

[//]: # (This is how you can make a comment that won't appear in the web page! It might be visible on some machines/browsers so use this only for development.)

[//]: # (Notice in the table of contents that [First Big Topic] matches #first-big-topic, except for all lowercase and spaces are replaced with dashes. This is important so that the table of contents links properly to the sections)

[//]: # (Leave this line here, but you can replace the name field with anything! It's used in the HTML structure of the page but isn't visible to users)
<a name='Topic 1'></a>

## 7.0 LEAVE THIS UNTIL WE ARE DONE so everyone has instructions
	
Here you can start to talk about the first topic of your notes. You can bold text like **this**, or italicize text like *this*. If you want to make a numbered list it's as easy as
1.  
2. 
3. 

- Bullet
- points
- are
- similar 

For a more detailed cheatsheet on the most important functionality of Markdown, check out this link https://wordpress.com/support/markdown-quick-reference/, which you can format in Markdown with your own [link title](https://wordpress.com/support/markdown-quick-reference/)


<a name='Subtopic 1-1'></a>
### Subtopic 1-1
You might want to include images in your notes, since Computer Vision as a field is blessed with tons of cool visualizations. Here's an example from the CS 231N notes page we included as a reference for you:

<div class="fig figcenter fighighlight">
  <img src="{{ site.baseurl }}/assets/examples/classify.png">
  <div class="figcaption">Put your informative caption here! If you really want to mess around with the classes in this div container then feel free, but inserting images just like this should work great!</div>
</div>

<a name='Subtopic 1-2'></a>
### Subtopic 1-2
Sometimes you might want to insert some code snippets into your notes. As an example, here's a snippet of python code taken from the CS 231N notes:
```python
Xtr, Ytr, Xte, Yte = load_CIFAR10('data/cifar10/') # a magic function we provide
# flatten out all images to be one-dimensional
Xtr_rows = Xtr.reshape(Xtr.shape[0], 32 * 32 * 3) # Xtr_rows becomes 50000 x 3072
Xte_rows = Xte.reshape(Xte.shape[0], 32 * 32 * 3) # Xte_rows becomes 10000 x 3072
```

<a name='Subtopic 1-3'></a>
### Subtopic 1-3
Sometimes you might want to write some mathematical equations, and LaTeX is a great tool for that! You can write an inline equation like this \\( a^2 = b^2 \\), or you can display an equation on its own line like this! \\[ a^2 = b^2 + c^2 \\]

You can also apply LaTeX syntax to label your equations and refer to them later! Here's the equation:

$$ \begin{equation} \label{your_label} a^2 = b^2 + c^2 + d^2 + e^2 \end{equation} $$

and here's a linked reference to it: \eqref{your_label}. For now, this configuration likes the \\"\\$\\$ equation stuff ... \\$\\$\\" syntax to have an empty line above and below it, but it displays the same anyway.

**For a guide on LaTeX syntax and how to write mathematical equations and formulas with it, check out [this link](https://www.overleaf.com/learn/latex/mathematical_expressions)** 

**Here's a short guide on how to use the basics of LaTeX**
- You've seen above the syntax to start and end an equation, so now let's work on what you fill in the middle
- You can make variables and expressions **bold** in equations too: \\(\mathbf{x} + y\\)
- Superscripts and subscripts are easy: use the ^ and _ symbol and bound your super/sub script by {} if it's more than one character. For example: \\(e^{-x+10}\\)
- Greek letters are also simple, use the \ character with their written name with optional capitalization, such as alpha or Alpha. For example: \\(\alpha + \beta + \gamma + \delta + \Gamma + \Delta\\). Not all capital greek letters work like this, but you can search online for solutions if this trick fails or reach out to the CA's. In general the \ character in LaTeX is the gateway to all kinds of special characters and functionalities.
- Sums and Products are really useful in Latex. You can use both superscripts and subscripts to mark the bounds: \\(\log(\prod_{i=0}^{2n}i^2) = \sum_{i=0}^{2n}\log (i^2)\\)
- Another useful trick is to write out a matrix or a vector in LaTex. There's a lot of customization you can do with this, so check out this [page](https://www.overleaf.com/learn/latex/Matrices) for more details. Here's some examples in our Markdown environment: 


$$\begin{bmatrix}
1 & 2 & 3\\
a & b & c
\end{bmatrix}$$

$$\begin{bmatrix}
1\\
2\\
3\\
\end{bmatrix}$$

$$\begin{bmatrix}
1 & 2 & 3\\
\end{bmatrix}$$

As with the labelled equations, it makes a difference whether the lines above and below the equation are blank, so keep that in mind while debugging! 

## 7.1 Optical Flow
This should give you the primary tools to develop your notes. Check out the [markdown quick reference](https://wordpress.com/support/markdown-quick-reference/) for any further Markdown functionality that you may find useful, and reach out to the teaching team on Piazza if you have any questions about how to create your lecture notes

## 7.2 Lukas-Kanade Method
This should give you the primary tools to develop your notes. Check out the [markdown quick reference](https://wordpress.com/support/markdown-quick-reference/) for any further Markdown functionality that you may find useful, and reach out to the teaching team on Piazza if you have any questions about how to create your lecture notes

## 7.3 Pyramids for Large Motion
### 7.3.1 Previous Assumptions and Large Motion
We made some key assumptions in the previous section about Lukas-Kanade Method, such as: 

**Small motion** - points do not move very far

**Brightness constancy** - projection of the same point is the same in every frame

**Spacial coherence** - points move like their neighbors

However we still need to consider cases with larger motion, more than one pixel. One potential idea that can help us is reducing resolution of the video, such that motion size is one pixel. 
<div class="fig figcenter fighighlight">
  <img src="{{ site.baseurl }}/videos/picture_7.3/IMG_3774.jpg">
</div>
Using that we can detect optical flow at the lower resolution image, and use that to build up back to higher resolution video. 

### 7.3.2 Optical Flow Estimation with Pyramids
We want to calculate optical flow in a coarse-to-fine approach. We first calculate the pyramid of the image, by converting image in lower resolution versions in multiple layers. As a result, motion that was 10 pixels in the high-resolution image, results in motion of 1.25 pixels in a lower resolution version. 
<div class="fig figcenter fighighlight">
  <img src="{{ site.baseurl }}/videos/picture_7.3/IMG_BECD9E44C44F-1.jpeg">
</div>

We run Lukas-Kanade on the low resolution image, because we can do it as now we satisfy small motion requirment. We use solution of that algorithm to upsample to the next layer with a little higher resolution. There we use L-K again but to detect residual movements that were not detected as a result of resolution decrease. We keep repeating that process until we get back to the original high resolution layer. 
<div class="fig figcenter fighighlight">
  <img src="{{ site.baseurl }}/videos/picture_7.3/IMG_AEBE3D9581A9-1.jpeg">
</div>

### 7.3.3 Optical Flow Results
We can see what happens when we don't use pyramids. Vectors in areas of large motion are very innacurate and fail to represent reality. 
<div class="fig figcenter fighighlight">
  <img src="{{ site.baseurl }}/videos/picture_7.3/IMG_78D6637B7B4A-1.jpeg">
</div>
Whereas on the picture where we use pyramids, we can see that vectors are properly alligned and calculated. 
<div class="fig figcenter fighighlight">
  <img src="{{ site.baseurl }}/videos/picture_7.3/IMG_8F39FB500A2B-1.jpeg">
</div>

## 7.4 Horn-Schunk Method
The Horn-Shunk method seeks to formulate optical flow as a global energy function minimization. So, we will minimize an energy function for an entire image. We can represent a global energy function with the following equation: \[ E = \int \int (I_xu+ I_yv + I_t)^2 + \alpha^2 (||\delta u||^2 + ||\delta v||^2)] dx dy \]
There are 3 major components of this equation to consider during our minimization process. \newline  
1. The 
\((I_xu+ I_yv + I_t)^2 \) component of the equation represents the brightness constancy, which we want to be equal to 0 in order to minimize our energy function. Thus, we want to calculate the values for u and v that minimize this term. \newline 
2. The \(||\delta u||^2 + ||\delta v||^2) \) component of the equation is the smoothness constant. By measuring the magnitude of the the gradient of u and the gradient of v, we are verifying that the changes between pixels are small. 
\newline 
3. \(\alpha \) is the regularization constant. A larger alpha enforces our smoothness constraint more than the brightness constraint. 

We can solve this minimization problem by deriving the equation with respect to \( u\) and \( v\) to get the following equations and can solve for u and v: 

\[ 0 = I_x(I_xu + I_yv + I_t) - \alpha \Delta u \]
\[ 0 = I_y(I_xu + I_yv + I_t) - \alpha \Delta v \]
In these equations, \( \Delta \) is the LaGrange operator, which can be measured using the following equation: 
\[ \Delta u(x, y) = \bar{u} (x,y) - u(x, y) \]
Where \( \bar{u} \) is the weighted average of u measured at a neighborhood around (x, y). 

We can substitute this definition for \( \Delta \) into our original 2 equations to get: 
\[ (I^2_x + \alpha^2)u + I_xI_yv = \alpha^2 \bar{u} - I_xI_t \]
\[ (I^2_y + \alpha^2)v + I_xI_yu = \alpha^2 \bar{v} - I_yI_t \]
These equations are linear in both u and v, so they can be solved analytically for each pixel individually. However, notice that the solution for \( u \) and  \( v \) for a pixel (x, y) depends on the optical flow values in a neighborhood around a (x, y), so we must iteratively update u and v once their neighbors have been updated. We get the following recursive equation to show this relationship: 
\[ u^{k + 1} = \bar{u}^k - \frac{I_x(I_x\bar{u}^k + I_y\bar{v}^k + I_t)}{\alpha^2 + I_x^2 + I_y^2}\]
\[ v^{k + 1} = \bar{v}^k - \frac{I_y(I_x\bar{u}^k + I_y\bar{v}^k + I_t)}{\alpha^2 + I_x^2 + I_y^2}\]
where we define \( \bar{u}^k\) and \(\bar{v}^k\) as the kth values of \(\bar{u}\) and \(\bar{v}\). 

The smoothness regularization constant is a sum of squared terms that we place in the expression to be minimized. It's important to note that in texture-free regions, there is no optical flow and on edges, points will flow to the nearest point, which solves the aperture problem. 


Michael Black extended the Horn-Schunk method. He replaced the quadratic regularization function term: 
<img width="297" alt="Screen Shot 2021-10-18 at 8 53 14 PM" src="https://user-images.githubusercontent.com/68766243/137841132-1b6adfeb-848b-4775-8f33-379894c25686.png">
with this function: 
<img width="302" alt="Screen Shot 2021-10-18 at 8 53 47 PM" src="https://user-images.githubusercontent.com/68766243/137841178-9a01add9-8349-4d4e-b6f9-42395fea01c8.png">



## 7.5 Applications
This should give you the primary tools to develop your notes. Check out the [markdown quick reference](https://wordpress.com/support/markdown-quick-reference/) for any further Markdown functionality that you may find useful, and reach out to the teaching team on Piazza if you have any questions about how to create your lecture notes
