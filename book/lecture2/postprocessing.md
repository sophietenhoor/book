---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.16.2
---

# Postprocessing for continuum fields

Up until now, we've only looked at discrete results: nodal displacement and support reactions. However, these results can be used to obtain the continuum field.

::::::{topic} Learning objective
You'll look into how to postprocess discrete results to obtain continuum results.
::::::

After solving for discrete nodal displacements, we can use the expressions derived in [](../lecture1/single_element.md) and [](./element_loads.md) to obtain continuous results. Remember than the nodal displacements are in the global coordinate system, which needs to be converted back into the local coordinate systems ($ \bar {\mathbf{u}}^e = \mathbf{T}\mathbf{u}^e$) to use the derived expressions.

If you want to create a figure which combines the internal forces / displacements of multiple elements, you need the results in the global coordinate system again. In the provided code, this is implemented with a boolean operation `global_c` in the class `elements.py` function `plot_moment_diagram` and `plot_displaced`. For example, the frame treated in [](../workshop1/Workshop_1_Apply.ipynb) gives the following displaced shape:

```{code-cell}
---
vscode:
  languageId: plaintext
---

```
