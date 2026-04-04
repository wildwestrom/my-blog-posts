---
title: Formally verifying the ideal transition curve 
subtitle: How I went from a hunch to a proof. The clothoid minimizes the first derivative of angular acceleration. Other transition curves minimize higher derivatives. What if I could minimize all higher derivatives to infinity?
date: 2026-04-04
tags: [curves, mathematics]
id: curves-part-2
---

Here's [part 1](/blog/007-curves-part-1) if you haven't read it yet. 

In part one I talked about how I came across the idea of a transition curve and their significance to track geometry. I loved how the clothoid could connect circles and lines smoothly.

<iframe src="https://www.desmos.com/calculator/cyddnvk61t?embed" width="500" height="500" style="border: 1px solid #ccc" frameborder=0></iframe>
 
In the previous article I mentioned that the goal of the clothoid was to transition between curvatures linearly. 'Linearly' is the key word here. This means that even if we reduce angular jerk, then we still have to deal with the same problem for its derivative, snap. There are in fact transition curves that reduce angular snap as well, but what if I could control all derivatives up to infinity?

## How do I go about creating a curve like this?

My first idea was to try to use Desmos to plot a $$C^\infin$$ curve. Wikipedia had a good article about [smoothness](https://en.wikipedia.org/wiki/Smoothness) which helped me with the intuition. That article linked to another one about [bump functions](https://en.wikipedia.org/wiki/Bump_function), so that gave me a good starting point for how to create these kinds of curves. 

I remebered the derivative of the exponential function was just the exponential function, so I figured I could start with a bump function exactly like the one from the Wikipedia article.


$$
f(x)={
	\begin{cases}
	\exp \left({\frac {1}{x^{2}-1}}\right),
	&{\text{ if }} |x| \lt  1,
	\\0,
	&{\text{ if }} |x| \geq 1,
	\end{cases}
}
$$

<iframe src="https://www.desmos.com/calculator/eugm6yhkyp?embed" width="640" height="480" style="border: 1px solid #ccc" frameborder=0></iframe>

From there, I knew I had to shift it toward the right and shrink it in half so it could represent curvature from 0 to 1.

$$
f(x)={
	\begin{cases}
	\exp \left({\frac {1}{(2x-1)^{2}-1}}\right),
	&{\text{ if }} 0 \lt x \lt 1,
	\\0,
	&{\text{ otherwise}}
	\end{cases}
}
$$

<iframe src="https://www.desmos.com/calculator/qixbzghlrd?embed" width="640" height="480" style="border: 1px solid #ccc" frameborder=0></iframe>

After doing some more iterations on it, I found another curve with the same properties but gives different higher derivatives.

$$
f_2(x)={
	\begin{cases}
	\exp \left({- \frac {1}{x(1-x)}}\right),
	&{\text{ if }} 0 \lt x \lt 1
	\\0,
	&{\text{ otherwise}}
	\end{cases}
}
$$

From here I had to somehow make a function that would increase monotonically. I tried playing around with these functions for awhile, but I couldn't figure anything out so I consulted an LLM. It showed me some integrals, so I figured I'd try playing around with integrating the function. I looked back at the Wikipedia article for the bump function and indeed it was staring me right in the face.

$$
g(x)=\int_{0}^{x}f(t)dt
$$

<iframe src="https://www.desmos.com/calculator/xgswj1i9l1?embed" width="640" height="480" style="border: 1px solid #ccc" frameborder=0></iframe>

The next thing to do was normalize it. I figured I could take the maximum (which just means setting the upper bound on the integral to any value
$$\ge 1$$) and then dividing $$g(x)$$ by that value. This gives us the final shape function $$h(x)$$. 

$$
C = \int_{0}^{1} f(t) dt
$$

$$
h(x) = \frac{g(x)}{C}
$$

<iframe src="https://www.desmos.com/calculator/32xahcv4bn?embed" width="640" height="480" style="border: 1px solid #ccc" frameborder=0></iframe>

Now the final step is to parameterize the curve. I should be able to go from any arbitrary starting curvature to any ending curvature along the length of the curve. This took me many hours and resulted in two curves with different slopes and higher derivatives. At this point though, this was enough to confirm my intuition that it's possible to build this kind of curve. Much later, I landed on this curvature function.

$$
\kappa(s)=R_1 + \Delta R \cdot h\left(\frac{s}{L}\right)
$$

where:  
- $$s$$ = arc length parameter with $$0 \le s \le L$$
- $$L$$ = total length of the transition curve
- $$R_1$$ = initial curve radius 
- $$R_2$$ = final curve radius 
- $$\Delta R := R_2 - R_1$$ = change in curvature


## Am I just fooling myself?

To be honest, a lot of the development of this curve was helped by using LLMs. That means for all I know, this curve could have none of the properties I want and only look like it does. So how would I go about proving my intuitions are correct? I would need a formal proof.

There's no way in hell I was going to try to prove the properties of the curve I wanted to manually. I make stupid mistakes all the time, and then I'd have to convince somebody better at math than me to check it. That's when I discovered proof assistants.

Proof assistants seemed too good to be true, but apparently provers like Lean 4 are widely used by mathematiticians to help check their proofs. I figured something relatively trivial like the properties of my curve could be checked in Lean as well.

I needed to prove four properties of the shape function $$h(x)$$.

1. Global $$C^\infty$$ smoothness for all real numbers.
2. Boundary values: $$ h(x) = \begin{cases}
	0,&
	x \le 0\\
	1,&
	x \ge 1
\end{cases} $$
3. Monotonicity along the shape function from $$[0,1]$$  
  (Increasing if $$\Delta R \gt 0$$ and decreasing if $$\Delta R \lt 0$$).
4. All derivatives vanish to zero at the endpoints.

These four properties, combined with global smoothness of $$h$$, imply that for all $$s \in \mathbb{R}$$ any arbitrary higher derivative of $$\kappa(s)$$ will be zero when $$s=0$$ or $$s=L$$.

How did I prove this? Did I go on a long journey of learning Lean 4?

No, I used LLMs.

## Vibe Proving

Turns out, all I had to do was understand just enough Lean to know if the final theorem is what I intend it to be, and then let the LLM fill in the rest. There is a pair of keywords in lean called `sorry` and `admit`. These keywords basically tell the proof assistant "trust me bro" and won't complain that you haven't proven the statements you're asserting.

```lean
def κ (s R L : ℝ) : ℝ :=
    F (s / L) * R

-- My curvature function is C^∞ continuous on [0, 1]
theorem κ_is_C_inf (R L : ℝ) : ContDiffOn ℝ ∞ (fun s => κ s R L) unitInterval := by
    admit
```

After burning over $100 in tokens over the course of a few weeks, using GPT-5 in Cursor, I managed to prove my curve was $$C^\infty$$ continuous. Once new smarter models came out, I could refactor the proof a lot and I ended up with the version that's now up on GitHub.

<https://github.com/wildwestrom/smoothstep-curve-proof>

In the end, as long as your assertion is what you think it is, you can just let the LLM generate whatever it wants until the prover is happy. I talked about this with some users of Lean on Slack, and they pretty much agreed with me.

Right now, the proof is complete. The next step is to figure out how to actually calculate cartesian coordinates for these curves, but that's another rabbit hole for another article.
