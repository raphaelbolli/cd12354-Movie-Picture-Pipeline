 CI (Continuous Integration) Workflows

  These workflows are designed to run checks on every pull request to ensure that new code meets the project's quality standards before it's merged into the main branch.

  1. Frontend CI (`.github/workflows/frontend-ci.yaml`)


   * Purpose: To validate the frontend code on each pull request.
   * Triggers:
       * When a pull request is opened or updated that targets the main branch and has changes in the frontend directory.
       * Can also be run manually.
   * What it does:
       * Lint & Test: It calls a reusable workflow (reusable-frontend-ci.yaml) to run the linter and the test suite in parallel.
       * Build: After the linting and testing pass, it builds the frontend Docker image to ensure it can be built successfully.
       * Report Status: It posts a comment on the pull request with the status of the linting, testing, and build steps.

  2. Backend CI (`.github/workflows/backend-ci.yaml`)


   * Purpose: To validate the backend code on each pull request.
   * Triggers:
       * When a pull request is opened or updated that targets the main branch and has changes in the backend directory.
       * Can also be run manually.
   * What it does:
       * Lint & Test: It calls a reusable workflow (reusable-backend-ci.yaml) to run the linter and the test suite in parallel.
       * Build: After the linting and testing pass, it builds the backend Docker image.
       * Report Status: It posts a comment on the pull request with the status of the linting, testing, and build steps.

  CD (Continuous Deployment) Workflows

  These workflows are designed to automatically deploy the application to a production environment (in this case, Amazon EKS) after changes are merged into the main branch.


  3. Frontend CD (`.github/workflows/frontend-cd.yaml`)


   * Purpose: To deploy the frontend application to the EKS cluster.
   * Triggers:
       * When code is pushed to the main branch.
       * Can also be run manually.
   * What it does:
       * Lint & Test: It runs the linter and the test suite.
       * Build & Push: If the tests pass, it builds the frontend Docker image, taking advantage of Docker layer caching to speed up the process. It then pushes the image to Amazon ECR (Elastic Container Registry).
       * Deploy: It uses kustomize and kubectl to deploy the new version of the frontend application to the EKS cluster.

  4. Backend CD (`.github/workflows/backend-cd.yaml`)


   * Purpose: To deploy the backend application to the EKS cluster.
   * Triggers:
       * When code is pushed to the main branch that has changes in the backend directory.
       * Can also be run manually.
   * What it does:
       * Test: It runs the backend test suite.
       * Build, Push, and Deploy: If the tests pass, it builds the backend Docker image with layer caching, pushes it to ECR, and then deploys the new version to the EKS cluster.

  Reusable Workflows


  To avoid duplicating code, we've created reusable workflows for the common CI steps.

   * `reusable-frontend-ci.yaml`: Contains the lint and test jobs for the frontend.
   * `reusable-backend-ci.yaml`: Contains the lint and test jobs for the backend.


  These reusable workflows are called by the main CI pipelines, which makes the pipelines easier to read and maintain.