---
layout: page
title: "How Shapeways’ Software Enables 3D Printing at Scale"
description: "While most news about the 3D printing industry focuses on advancement in hardware and materials, software has played a crucial role in the democratization of 3D printing. Companies like Shapeways have delivered software to generate 3D files, prepare and optimize them for printing, and manufacture and distribute."
category: software
tags: shapeways, 3d printing
---
{% include JB/setup %}
Originally posted on the [StackOverflow Blog](https://stackoverflow.blog/2020/02/06/how-shapeways-software-enables-3d-printing-at-scale/)
<br><br/>
A decade or two ago, getting a custom part manufactured required you to have your own workshop or to make a visit to a factory floor. Today, you can create your own 3D model, upload it to a website, and have a functional product delivered to your door within a few days—a turn around time unimaginable just 20 years ago.

While most news about the 3D printing industry focuses on advancement in hardware and materials, software has played a crucial role in the democratization of 3D printing. Companies like Shapeways have delivered software to generate 3D files, prepare and optimize them for printing, and manufacture and distribute.

Shapeways’ primary technology offerings can split into two categories—the ability to upload, repair, price, and purchase 3D models in a variety of materials, and back-end systems driving the manufacturing, distribution, and fulfillment of our orders at a global scale. I’m going to discuss three distinct pieces of software that occur in separate steps in the buying process: one that help customers upload designs and make purchases: Model Processing; one that securely shows the customer the final printable model: ShapeJS; and one that helps us manufacture, distribute, and fulfill those design purchases: Inshape. 


### Processing Customer Models

Our first contact with a customer’s order is when they upload a 3D model. We have no control over the quality and printability of the model, so our software repairs errors during model generation where it can and analyzes their printability in a wide variety of materials. This is a very compute-heavy process—we calculate the model surface area and volume, determine the number of parts that the model is composed of, and examine the model for errors and attempt to repair them, all within a mean time of 25 seconds. 

In order to deliver these results, we needed to build a system that leverages parallelism and provides easy scalability to handle fluctuations in load without breaking our SLA. To start, we decided to build individual services that are each responsible for evaluating different components of printability. These services fall into three categories: model validation, model pricing calculation, and model repair. 

Model validation services are charged with validating that the model can be printed in the first place. These checks make sure that the file is valid, that we can process it, and that the model is manifold—water-tight. Pricing calculation services are responsible for generating pricing components of our models, including but not limited to volume, part count, and surface area. Finally, fixing services help us repair our customer models and ensure that they’re printable. This includes steps like repairing meshes, decimating models to reduce triangle counts, and fixing inverted matrices on the model.

Some of these services generate new files to facilitate printing, production, or image display on our site. These services are mostly implemented in Java (the language of choice for our 3D tools team), though some have been re-implemented in [CUDA](https://en.wikipedia.org/wiki/CUDA) to run directly on GPUs for improved performance.

Once we implemented these services, we had to string them together into what we call a Model Processing Pipeline. This pipeline is a chain of the services described above that takes in a 3D file at the start and outputs a fully priced, printable, rendered 3D Model on Shapeways.com. We defined these pipelines in another service called the Director, which is effectively a [directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph) of model processing services. 

To ensure that our services remain available at all times, we needed another layer to manage traffic. So rather than pointing directly to the services themselves, these steps indicate [ActiveMQ](https://activemq.apache.org/) queues, which act as load balancers for a pool of services that can handle a pipeline step. Using ActiveMQ in this fashion allows us to run our services anywhere in our hybrid cloud, as well as to scale at the service level rather than the pipeline level. Need more mesh repair compute in Europe? Spin up more instances in either our data center or AWS and subscribe them to the queue. With this capability, we’re able to optimize our model processing compute power for both user experience and cost, providing a best-of-both-worlds experience for Shapeways and our customer base.

### Secure Real-Time Rendering

Once we’ve got a printable model, we have to actually display it to the user in 3D for their review. Our uploaders have a chance to perform a visual review of key geometry components to be sure that our repair process didn’t alter the model. To assist with this review, we provide visualizations to help users understand potential problem areas with their models, such as thin walls or unintentionally connected parts. These visualizations are rendered directly on the users own computer graphics card, ensuring fast and reliable performance as they review their files. 

Visualizing the customer’s own model is easy enough; they own the model, so they can render it on their system. But in addition to the online 3D printing service, Shapeways also provides a marketplace for our uploaders to open a shop and sell prints of their models. Our customers purchasing from our uploaders want to see a three-dimensional view of the models that they’re thinking about purchasing. 

Here’s where the problem occurs—the industry-standard visualizations rendered on the consumers computer require us to transmit the entirety of the model file data to that users video card via WebGL. Bad actors can easily lift this 3D file from their own graphics cards, thereby stealing the model from the original uploader without paying for it, a massive violation of their IP rights. Shapeways takes the IP rights of all of our customers extremely seriously, so even the possibility of this edge case was deemed too risky to let stand. Therefore, we decided to build our own IP-safe 3D viewer to allow customers to view parts in 3D. 

Enter [ShapeJS](https://shapejs.shapeways.com/v2/examples). ShapeJS is a JavaScript-based tool we created to generate and display 3D geometry in real-time on the user’s browser. We created ShapeJS as a research project—could we create a web-based, voxel-backed, IP-safe, 3D design interface for developers as opposed to for those with background in 3D modeling? The back end of ShapeJS is built entirely in CUDA and runs directly on a server GPU for high-performance rendering.

ShapeJS is a parametric design tool, which means that you describe the geometry you want to create in code, and the tool converts this into printable 3D geometry. For example, here’s a simple ShapeJS function to create a [gyroid](https://en.wikipedia.org/wiki/Gyroid) extruded through a sphere. 

```javascript
 function main(args) {
    var radius=25*MM;
    var sphere=new Sphere(radius);
    var gyroid=new VolumePatterns.Gyroid(args.period*MM,args.thickness*MM);
    var intersect=new Intersection();
    intersect.setBlend(2*MM);
    intersect.add(sphere);
    intersect.add(gyroid);
    var s=26*MM;
    return new Scene(intersect,new Bounds(-s,s,-s,s,-s,s));
}
```

