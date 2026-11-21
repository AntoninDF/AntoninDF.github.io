# Fake orbitting light

## The expected effect

Great lighting can add so much value to your 3D assets, but it can also be a bit tricky to get a precise lighting on some specific assets. For example, rim lights or hair highlights can be faked through shaders for better artistic control. In this article, we'll see how we can simulate the effect of a light orbitting around the mesh.

![Image of the final shader result](MISSING)

## How it works

We'll treat the light as if it was a directional light. Since directional lights don't need positions, we only need to find it's direction. What's convenient about it is that it works for any mesh out of the box, whereas simulating a point light, for instance, would require a position. But since we do not have data on the whole mesh in the fragment shader, we wouldn't be able to prevent the point light intersecting with the mesh, and creating undesired effects (unless we put it far enough, but it would yield an inconsistent lighting).

So once we find the directional light's direction, we must apply it to the surface's color.

## The math behind

Rotations are all about trigonometry. To generate a rotating direction vector, we make use of Sine and Cosine, and make sure that they are out of sync so that the rotation appears random.
Using the time variable T, we can take for example :

```L = Vector3(Sin(T*3.5), Cos(T*4.5), Sin(T*5.5))```

Then normalize it to make sure we always have a unit vector.

Now we have our light direction, we can simply compute it's contribution to the surface using simple phong lighting.

Given the surface normal N, we compute the incident light I by calculating the dot product of the normal with the negated light direction vector :

```I = Dot(N, -L)```

We get I in a [-1;1] range, since both N and L are of unit length.
It should then be clamped between 0 and 1 to avoid negative light contribution.


WIP