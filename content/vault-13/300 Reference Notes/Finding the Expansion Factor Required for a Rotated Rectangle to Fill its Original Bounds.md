[[Algorithms]], [[Math]]

I found a very well formed answer to this problem on the Math Stack Exchange.

Here is the URL: https://math.stackexchange.com/questions/2773788/rectangle-rotation-and-scale

## Contents

Introduce two notations: vector coordinates with respect to the center of the rectangles, $C$, will look like this:

$$\left[\begin{matrix}x\\y\end{matrix}\right]_C,$$

and if we let $O$ be the upper-left corner of the un-rotated frame, we will denote coordinates with respect to that reference as

$$ \left[\begin{matrix}x\\y\end{matrix}\right]_O. $$

Now, to rotate something clockwise through an angle \alpha, we multiply coordinate vectors by the rotation matrix

$$ R_{\alpha}=\left[\begin{matrix}\cos(\alpha) &\sin(\alpha)\\-\sin(\alpha)&\cos(\alpha)\end{matrix}\right]. $$

This only applies in the $C$ reference frame, since, as per your comment, you're only rotating about $C$.

Now, you have posed two problems: one is to find the magnification such that the rotated frame fills the unrotated frame. The other is to find the coordinates of the upper-left coordinate of the rotated frame with respect to $O$. The second problem is easier, I will tackle that first.

Let w be the width of the frame, and $\ell$ the height. Let $O_C=\left[\begin{matrix}-w/2 \\ \ell/2\end{matrix}\right]$ be the coordinates of $O$ in $C, P_C$ the coordinates of the upper-left corner of the rotated frame in $C$, and $P_O$ the coordinates of the upper-left corner of the rotated frame in $O$. By the properties of vector addition, we have that $O_C+P_O=P_C$. The target variable is $P_O$, so we have that

$$\begin{align*}P_O&=P_C-O_C\\&=R_{\alpha}O_C -O_C\\&=R_{\alpha}O_C-IO_C\\&=(R_{\alpha}-I)\,O_C\\ &=\left[\begin{matrix}\cos(\alpha)-1 &\sin(\alpha)\\-\sin(\alpha) &\cos(\alpha)-1\end{matrix}\right]\left[\begin{matrix}-w/2\\\ell/2\end{matrix}\right]\\ &=\frac12\left[\begin{matrix}w(1-\cos(\alpha))+\ell\sin(\alpha)\\ -w\sin(\alpha)-\ell(1-\cos(\alpha))\end{matrix}\right]\end{align*}$$

As for the magnification, this is a tricky problem to solve. Let $\theta=\arctan(\ell/w)$, and let $\varphi=\arctan(w/\ell)$. I am going to make the assumption that $w>\ell$, so that $\varphi>\theta$. For rotation angles from 0 to $\pi/2$, there are three entirely different regimes, and we have to compare the long and short sides of the rotated frame to different sides of the fixed frame depending on which regime we're in. Here's a table of comparisons:

$$\begin{array}{|c|c|c|} \hline &\textbf{Rotated Long} &\textbf{Rotated Short} \\ \hline 0\le\alpha\le\theta &\text{Fixed Long} &\text{Fixed Short}\\ \hline \theta\le\alpha\le\varphi &\text{Fixed Long} &\text{Fixed Long} \\ \hline \varphi\le\alpha\le\pi/2 &\text{Fixed Short} &\text{Fixed Long} \\ \hline \hline \end{array}$$

Let $s=\sqrt{w^2+\ell^2}$ be the diagonal length.

**Case 1:** $0\le\alpha\le\theta$. Let $\beta=\varphi-\alpha$. This will be the angle a diagonal of the fixed frame makes with a rotated "vertical" line. The perpendicular distance $\ell'/2$ from the rectangle center to a line going through the corner of the rectangle, along the "vertical" of the rotated frame, would then be given by

$$\frac{\ell'}{2}=\frac{s}{2}\,\cos(\beta),$$

or $\ell'=s\cos(\beta),$ and hence the magnification required in the "vertical" direction would be given by

$$\frac{\ell'}{\ell}=\frac{s\cos(\beta)}{\ell}.$$

The magnification required in the "horizontal" direction we analyze as follows. Let $\gamma=\theta-\alpha$ be the angle a diagonal in the fixed rectangle makes with the "horizontal" of the rotated frame. The perpendicular distance $w'/2$ from the center to the corner, along the "horizontal" of the rotated frame, would be given by

$$\frac{w'}{2}=\frac{s}{2}\,\cos(\gamma),$$

or $w'=s\cos(\gamma)$, and hence the magnification required by the "horizontal" direction would be given by

$$\frac{w'}{w}=\frac{s\cos(\gamma)}{w}.$$

The magnification m you should take would simply be

$$\begin{align*}m&=\max\left(\frac{s\cos(\beta)}{\ell},\frac{s\cos(\gamma)}{w}\right)\\ &=s\max\left(\frac{\cos(\beta)}{\ell},\frac{\cos(\gamma)}{w}\right)\\ &=\sqrt{w^2+\ell^2}\max\left(\frac{\cos[\arctan(w/\ell)-\alpha]}{\ell},\frac{\cos[\arctan(\ell/w)-\alpha]}{w}\right). \end{align*}$$

For Cases 2 and 3, although your \gamma and \beta angles will go negative, the formula is still valid, with the additional constraint of magnitudes. That is, your final answer is

$$m=\sqrt{w^2+\ell^2}\max\left(\frac{|\cos[\arctan(w/\ell)-\alpha]|}{\ell},\frac{|\cos[\arctan(\ell/w)-\alpha]|}{w}\right).$$