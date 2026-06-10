
# Small example of a multi-job pipeline with test in a Docker container


### Pipeline execution process 

```mermaid
flowchart TD
    A["Triggering parentCaller.yml"] --> B["prepare-allure-context"]
    B --> C["Get GitHub workflow metadata"]
    C --> D["Download allurectl"]
    D --> E["Gathering pipeline context and pack it in encoded ALLURE_CI_ENV"]
    E --> F["Upload artifact with packed parent pipline context: allure-ci-env"]
    F --> G["Triggering childWithTestExec.yml"]
    G --> H["Call reusable child workflow"]
    H --> I["Checkout repository"]
    I --> J["Download artifact: allure-ci-env"]
    J --> K["Run Docker container: python:3.12"]
    K --> L["Install pytest and allure-pytest"]
    L --> M["Run allurectl watch -- pytest"]
    M --> N["Send results to Allure TestOps"]
    M --> O["Upload allure-results artifact"]
```
### 422 Error When Triggering a Pipeline from TestOps

A detailed explanation of this error can be found in the following documentation section:

[Error 422 when triggering GitHub workflow from Allure TestOps interface](https://docs.qameta.io/allure-testops/integrations/github/#error-422-when-triggering-github-workflow-from-allure-testops-interface)

In the pipeline used in this example, the same issue may occur if the `SECOND_DUMMY_INPUT` parameter defined in `parentCaller.yml` is marked as required, but is either:

* Missing from the job configuration in Allure TestOps
* Does not have a default value configured.

<img width="1072" height="445" alt="image" src="https://github.com/user-attachments/assets/656f15cb-e85d-4dc4-bb1c-5437651b6a17" />

#### Reference Configuration

This configuration will successfully trigger the pipeline because the required input is properly configured.

<img width="841" height="599" alt="image" src="https://github.com/user-attachments/assets/c8c4010a-7d8d-482b-b6dd-a777d58d2edb" />

### Examples That Cause the Error

In the example below, a 422 error will occur because `SECOND_DUMMY_INPUT` is missing from the job configuration.

<img width="836" height="510" alt="image" src="https://github.com/user-attachments/assets/8df892bf-2eae-4e11-b9d8-bb24f0872f1c" />

---

In the example below, a 422 error will occur because no default value is configured for `SECOND_DUMMY_INPUT`, even though the parameter is marked as required on the GitHub side.

<img width="835" height="594" alt="image" src="https://github.com/user-attachments/assets/3470e38c-aa8d-43f8-bbf7-7d1aacbae87f" />
