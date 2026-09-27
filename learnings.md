# My Learning Log

## what is pytorch
*2026-09-26 09:50*

pytorch is an open-source deep learning framework originally built by meta's ai research lab and now run by the pytorch foundation under the linux foundation. its two core pieces are tensors, which are like numpy arrays that can run on a gpu, and autograd, which automatically computes gradients so you can train neural networks by adjusting weights to reduce error. pytorch uses eager execution, meaning code runs line by line like normal python, so you can print values, use regular loops and if statements, and debug with standard tools. this is a big reason it took over research compared to older static-graph frameworks like early tensorflow, and most papers and open models (including much of the hugging face ecosystem) are written in it. a good learning path is: tensors and basic ops, then autograd, then building models with torch.nn and training them with torch.optim, then writing training loops with DataLoader, and later things like fine-tuning pretrained models, torch.compile, and pytorch lightning. the official 'learn the basics' tutorial on pytorch.org is a solid starting point.

## simple neural network in pytorch
*2026-09-26 09:50*

a minimal pytorch example: generate 1000 random 2d points and label each 1 if it lies inside the unit circle, else 0. the model is nn.Sequential(nn.Linear(2, 16), nn.ReLU(), nn.Linear(16, 1)), where Linear is a fully connected layer and ReLU adds the non-linearity needed to learn a curved decision boundary. the loss is nn.BCEWithLogitsLoss, used for binary classification; it applies sigmoid internally, so the model outputs raw logits. the optimizer is torch.optim.Adam(model.parameters(), lr=0.01). the training loop repeats the same five steps found in almost every pytorch project: forward pass (logits = model(X)), compute loss, optimizer.zero_grad() to clear old gradients, loss.backward() so autograd computes gradients, and optimizer.step() to update weights. for evaluation, wrap code in torch.no_grad() to turn off gradient tracking, then threshold logits at 0 to get predictions; this example reaches roughly 95%+ accuracy after 500 epochs. the natural next step is rewriting the model as a class that extends nn.Module, which is how most real pytorch code is structured.

## 3d printing software and beginner designs
*2026-09-26 10:47*

3d printing software falls into three stages: modeling (making the design), slicing (turning the model into gcode the printer understands), and extras (repair, remote control, model libraries). Modeling tools: Tinkercad is free, browser-based, and drag-and-drop, which makes it the best starting point. Fusion (Autodesk) and Onshape are parametric CAD tools for precise functional parts; Fusion has a free personal-use license and Onshape runs fully in the browser. FreeCAD is a free, open source parametric CAD tool with a steeper learning curve. Blender is free and suited to organic or artistic models like figurines, but not precise mechanical parts. OpenSCAD lets you build models by writing code. Slicers: Ultimaker Cura (free, works with most printers), PrusaSlicer (free, works with non-Prusa printers too), Bambu Studio (for Bambu Lab printers), and OrcaSlicer (open source fork of Bambu Studio that supports many printers). Extras: Meshmixer or Microsoft 3D Builder repair broken STL files, OctoPrint allows remote printer control via a Raspberry Pi, and Printables, Thingiverse, and MakerWorld offer ready-made models. Suggested learning path: download a model, slice it in Cura or OrcaSlicer, and print it, then learn Tinkercad and move to Fusion or Onshape for precise parts. Simple beginner designs to try: a name keychain, phone stand, cable clip, pen holder, coaster, bookmark, headphone hook, a small box with a lid, a desk nameplate, and a cookie cutter. For a box lid, add about 0.2-0.4 mm of clearance so it fits. Design tips: keep flat surfaces on the bottom to avoid supports, avoid overhangs steeper than about 45 degrees, keep walls at least 1.2-2 mm thick, and start with small prints. A good progression is the keychain first, then the box with a lid, since getting the fit right teaches tolerances.

## python decorators
*2026-09-26 17:34*

what it is: a decorator is a function that takes another function, wraps extra behavior around it, and returns a new function. it lets you reuse logic like logging, timing or auth checks without editing the original function.

why it works: in python, functions are objects. you can pass them into other functions, return them, and assign them to variables, which is exactly what a decorator does.

the @ syntax: writing @my_decorator above def greet() is just a shortcut for greet = my_decorator(greet). inside, the decorator defines a wrapper function that runs code before and after calling the original, then returns wrapper.

making it work for any function: use *args, **kwargs in the wrapper so it accepts any arguments, and return the original function's result so nothing gets lost. add @functools.wraps(func) on the wrapper so the function keeps its real name and docstring.

where you see them: @property, @staticmethod, @classmethod in classes, @app.get() in fastapi, @lru_cache for caching, and @pytest.fixture in tests. the next level is decorators with their own arguments like @retry(times=3), which adds one more layer of wrapping.

## why octopuses have three hearts
*2026-09-27 16:41*

octopuses have three hearts. two of them, called branchial hearts, pump blood through the gills to pick up oxygen. the third, the systemic heart, pumps that oxygenated blood to the rest of the body. their blood is blue because it uses hemocyanin, a copper-based protein, instead of the iron-based hemoglobin humans use. hemocyanin carries oxygen less efficiently, which is part of why they need the extra pumping power. a weird detail: the systemic heart actually stops beating while the octopus swims, which is one reason they prefer crawling and tire out quickly when swimming.
