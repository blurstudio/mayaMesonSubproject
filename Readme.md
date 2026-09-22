A Meson subproject that will set up a dependency for Autodesk Maya.

## Using this in another project

Add it to your project as a Meson subproject via a `.wrap` file. Copy
[maya.wrap.example](maya.wrap.example) to `subprojects/maya.wrap` in your
project:

```ini
[wrap-git]
url = https://github.com/blurstudio/mayaMesonSubproject.git
revision = main
depth = 1

[provide]
maya = maya_plugin_dep
maya-plugin = maya_plugin_dep
maya-core = maya_core_dep
maya-qt = maya_qt_dep
maya-numpy = maya_numpy_dep
maya-alembic = maya_alembic_dep
maya-imath = maya_imath_dep
maya-ufe = maya_ufe_dep
maya-opengl = maya_opengl_dep
```

Then request the dependency like any other in your `meson.build`. Because of
the `[provide]` section, Meson will automatically fall back to this
subproject when the dependency isn't found on the system:

```meson
maya_dep = dependency('maya')
```

For the optional dependencies (`maya-qt`, `maya-numpy`, `maya-alembic`,
`maya-imath`, `maya-ufe`, `maya-opengl`), the underlying feature options
default to `disabled` (or `auto` for `maya-opengl`) in this subproject, so
request them with `default_options` to turn the corresponding option on when
this project is pulled in as a fallback:

```meson
maya_qt_dep = dependency('maya-qt', default_options: ['maya_link_qt=enabled'])
```

Other options (`maya_version`, `maya_devkit_base`, `maya_custom_install_path`,
`maya_qt_extra_includes`) can be set the same way via `default_options`, or
from your top-level project since they're declared with `yield: true`.
