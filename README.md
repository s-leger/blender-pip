# blender-pip

#### What does it do ?
Allow easy pip modules setup

#### Common issues with pip module setup
* Global python site packages folder is likely to be read only 
* Break your software by setting up dependency, overriding stock modules
* System may not be able to build modules

#### Solutions
* Setup into user site package
* Never install deps (you have to handle your deps by hand)
* Only download and install from wheels

By default Pip class use ```-user --only-binary all --no-deps``` options to call pip.



### Usage

#### Basic methods
```python
Pip.install("scipy==1.2.1")
Pip.uninstall("pyproj==2.1.3")
```

#### Upgrade pip  
```python
Pip.upgrade_pip()
```


#### Dependency check
You have to check for your modules dependency, and setup by hand according your needs.  
Also ensure there are wheels available for all target platforms.  
```python
python_version = Pip.python_version()
if python_version.minor == 6:
```

#### Setup modules 
see https://pip.pypa.io/en/stable/reference/pip_install/    
```python
Pip.install("scipy==1.2.1")
Pip.install("pyproj==2.1.3")
Pip.install("Pillow==6.0.0")

# Sample usage in a blender operator

def execute(self, context):
    # Pyside2 require shiboken
    # override default options to skip --no-deps
    # we may setup both by hand instead
    res, msg = Pip.install("pyside2", "--user  --only-binary all")
    if res:
        status = {'SUCCESS'}
    else:
        status = {'ERROR'}
    self.report(status, msg) 
    return {'FINISHED'}

```

#### Advanced options
Raw pip calls using static _cmd method
```python
Pip()._cmd("install", options, module)
```

#### Integration into addons
From version 2.83, blender will no more expose user site-package in path.
Ensure user site package method detect site package and add to sys.path when missing.
Call this method from your init, before custom module(s) import

```python
Pip.ensure_user_site_package()
```


#### Manual setup of modules
In blender python console
```python
from blender_pip import Pip
Pip.install("module_name=version")
```
