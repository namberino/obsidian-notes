# What are vectors, really?

Thinking about vectors as list of numbers that happens to have a nice visual representation is concrete. It's nice to work with, and makes it easy to do higher dimension vectors like 4D or 5D. However, some ideas about vectors seem to really don't care about the coordinate system and are inherently spatial.

So if vectors are not fundamentally lists of numbers, and if vectors' underlying essence is more spacial, then what are they, really?

There are a lot of vector-ish things in math (eg. functions, derivatives, etc). As long as you deal with a set of objects where there's a reasonable notion of scaling and adding, all the tools in linear algebra should be able to applied to handle these sets.

In the modern theory, the form that vectors take doesn't really matter. So long as there's some notion of adding and scaling vectors.

# Vector spaces

The sets of vector-ish things like arrows, or functions, or lists of numbers, are called *vector spaces*.

The vector spaces have a list of rules that they need to abide by, called *axioms*. In modern linear algebra, there are 8 axioms that any vector spaces must abide by.

The 8 axioms:
$$
\begin{aligned}
&\text{1. } \vec{u} + (\vec{v} + \vec{w}) = (\vec{u} + \vec{v}) + \vec{w}
\\
&\text{2. } \vec{v} + \vec{w} = \vec{w} + \vec{v}
\\
&\text{3. } \text{There is a vector 0 such that } 0 + \vec{v} = \vec{v} \text{ for all } \vec{v}
\\
&\text{4. } \text{For every vector } \vec{v} \text{ there is a vector } -\vec{v} \text{ so that } \vec{v} + (-\vec{v}) = 0
\\
&\text{5. } a(b\vec{v}) = (ab)\vec{v}
\\
&\text{6. } 1 \vec{v} = \vec{v}
\\
&\text{7. } a (\vec{v} + \vec{w}) = a\vec{v} + a\vec{w}
\\
&\text{8. } (a + b) \vec{v} = a\vec{v} + b\vec{v}
\end{aligned}
$$