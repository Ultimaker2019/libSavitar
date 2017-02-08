Introduction
------------

libSavitar is a c++ implementation of 3mf loading with SIP python bindings.

Build instructions Windows with MSVC
------------------------------------

Make sure you have run something like:

“c:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\vcvarsall.bat" amd64

Next, git clone libSavitar and from the directory:

```dos
mkdir build
mkdir install_dir
cd build
cmake -DCMAKE_INSTALL_PREFIX=../install_dir -DBUILD_STATIC=ON -DCMAKE_BUILD_TYPE=Release -G "NMake Makefiles" ..
nmake
nmake install
```

Now copy the site-packages contents from the install_dir into your site-packages dir.

Usage
-----

Reading 3mf's 3dmodel.model file.

```python
file_name = "project.3mf"
archive = zipfile.ZipFile(file_name, "r")
parser = Savitar.ThreeMFParser()
scene_3mf = parser.parse(archive.open("3D/3dmodel.model").read())
for node in scene_3mf.getSceneNodes():
    my_node = convertSavitarNodeToMyNode(node)
```

Writing

```python
archive = zipfile.ZipFile(stream, "w", compression = zipfile.ZIP_DEFLATED)
model_file = zipfile.ZipInfo("3D/3dmodel.model")
savitar_scene = Savitar.Scene()
#construct your node
savitar_scene.addSceneNode(savitar_node)
parser = Savitar.ThreeMFParser()
scene_string = parser.sceneToString(savitar_scene)
archive.writestr(model_file, scene_string)
```