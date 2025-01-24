## Graded assignment

When you've finished the workshops, you can start with the graded assignment. The process is very similar to the workshops, but now there's a deadline and you're required to write a report. You're going to solve a to be provided structural model for displacements and internal forces using the Matrix Method.

```{figure} figures/graded_assignment.svg
```

This model is based on the bridge crossing the L723 in Walldorf, Germany:

```{figure} https://www.wiwa-lokal.de/wp-content/uploads/2017/11/Br%C3%BCcke.jpg
---
width: 400px
class: dark-light
---
{cite:ts}`bridge` (Foto: Pfeifer)
```

Please open the assignment from GitHub Classroom (to be provided too) to work on it locally. The solution won't be provided.

First, analyse the model and the questions you're required to answer:
1. Explain what effect hinges have on the matrix method. The following questions might help you:
   - Which deformations would you expect for an element with hinges on both ends?
   - Which internal forces would you expect for an element with hinges on both ends? How do those forces related to forces in the global coordinate system?
   - Which degrees-of-freedom are relevant for an element with hinges on both ends?
   - Can you calculate the rotation of hinged nodes? And if so, what is the meaning of those rotations?
   - What is the difference in terms of degrees of freedom for nodes which contains hinged and clamped connections, compared to nodes which are fully hinged or fully clamped?
   - Can you reuse the element classes derived before?
   - Can you find an exact solution to this model using the matrix method?
2. Explain in words and math how you adapted/added code and/or procedures to solve this structure including hinges.
3. Describe alternatives you considered for your implementation in the previous steps.
4. Explain why your sanity checks prove that all of your code implementations are correct.
5. Make a table of all nodal displacement and show the displaced structure in a figure. Indicate how you identify nodes.
6. Show the moment diagram of the structure in a figure.
7. Provide a figure of a free body diagram of the full structure in which you show all the forces working on the structure (including support reactions) with numerical values from your code. This specific figure can be hand drawn.
8. Provide a figure of a free body diagram of the the indicated node with numerical values from your code. This specific figure can be hand drawn. If you implement code for this in the matrixmethod package, make sure to perform sanity checks.
9. Comment on any potential mistakes you observed in your final answers.
10. If you had the time to expand this Matrix Method with an additional feature, what would that be?

Then, add potential new implementations to your code in `./matrixmethod/`. Please note that `./matrixmethod/` doesn't include any solutions from the workshops. If you've made new implementations, provide sanity checks to your implementations in `Graded_Implement.ipynb`. Then, solve and postprocess the problem in `Graded_Apply.ipynb`. Add a report in `.pdf` or `.md` format in which you included answers and reasoning for all the questions. Make sure all the values/figures you use in the report are solved/created with your code. Except for question 7 and 8: the free-body-diagrams can be hand-drawn. Furthermore, you're expected to provide to logically organise any auxiliary files you may use.

The deadline of the assignment is April 18th, 23:59, although you’re encouraged to finish it directly after completing workshop 2. Doing so allows you to split the workload evenly. Commit your repository to the provided GitHub Classroom repository to hand in your assignment. Your latest commit before the deadline in the `main` branch will be graded. Incomplete assignments will be graded with a 1. The full solution won't be provided.

You can take the resit of this assignment in Q4. If you choose to do so, you can improve your first submission by resubmitting to the same repository. The deadline of the resit is June 20th, 23:59. For more information, see [](./course_information.md)