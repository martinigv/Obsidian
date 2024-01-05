To have an idea of the skills you need, let me give you a quick breakdown of the path you should take to develop a game engine:

Path To Develop a Game Engine

1. Develop a Math Engine
2. Develop the Rendering Engine
3. Develop the Physics Engine
4. Implement the Collision-Detection System
5. Add secondary systems, such as Camera System, Particle Systems

So, what set of skills do you need to develop each component? Here they are:

Skills to Develop the Math Engine

You are going to need to become acquainted with Linear Algebra concepts. Space Transformations are operations you will use many times, so you must understand them well. An operation that is used often, at least by me, is the Dot Product. It is a simple Vector Operation, but surprisingly very useful.

Although you can develop a math engine with just Vectors and Matrices, at one point, you may want to consider using Quaternions, and posibly, Double Quaternions. They are a lot faster and consume less memory than matrices.

As you progress with your game engine, you are going to need to implement operations related to Computational Geometry, for example, finding the point inside a Tetrahedron.

So, when it comes to developing the Math Engine, the primary skills required are a good grasp of Linear Algebra and Computation Geometry.

Skills to Develop a Rendering Engine

The primary skill you need to implement your Rendering Engine is a firm grasp of Computer Graphics. Focus your energy on understanding the Graphics Rendering Pipeline. Learn how the Vertex and Fragment Shaders work. Learn how the GPU receives data required for rendering.

Once you know Computer Graphics, then it is time to learn about the Graphics Library you want to use. I use Metal, but you may want to use OpenGL or Vulkan. At this stage, I strongly recommend to experiment a lot and do as many projects as possible. The knowledge you build in this stage will be beneficial once you start implementing the Rendering Engine.

Skills to Develop a Physics Engine & Collision Detection System

The primary skill you need to develop these systems is to be able to translate algorithms into code. However, it is not just writing code; it is writing efficient code.

You are going to be exposed to several algorithms, such as the Runge-Kutta 4th Order Algorithm used for Numerical Integration. The GJK Algorithm used for Collision Detection, The BVH Algorithm, used to parse the space in your game.

Keep in mind that some of these algorithms are hard to grasp, and it may take you some time to write them.

Skills to Develop Secondary Systems

Once you have developed the systems mentioned above, you are on the other side of the hill. And it is when the fun begins. You get to write a Camera System (1st person, 3rd person cameras). You get to implement a Particle System for explosion effects, etc. You get to write an AI system, etc. Per my experience, there is something to fix, improve, or add to my game engine.

Thus, the traits that you need is a strong desire to Keep Learning and Continuously be Improving.

Programming Skills

On top of all these, you are going to need a firm grasp of Programming Principles, Design Patterns, and Algorithms. You don't have to be an expert when you start but do know the basics. For example, if you are going to use C++, then make sure to understand the basic principles of Object-Oriented Languages. Make sure to read on Inheritance, Polymorphism, Encapsulation.

As you progress, you will need to use Design Patterns. You want the architecture of your engine to be modular, flexible, adaptable, and maintainable.

I hope this helps.
