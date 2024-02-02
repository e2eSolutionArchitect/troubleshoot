# Git Workflow not showing 'Run Workflow' even after adding 'workflow_dispatch'

- I have pushed the yml . the workflow to be trigged manually. But it is not showing at all.
- I don't see the 'Run Workflow' option even my workflow has workflow_dispatch event

## Resoulution
- For both the cases make sure you have pushed your yml in main branch. Not any other branch.
- Manu cases the workflow doesnt show up unless it is in main branch. 

Please [check here](https://github.com/e2eSolutionArchitect/sonarcloud-gitaction-terraform-scan) for gitaction pipeline reference.
