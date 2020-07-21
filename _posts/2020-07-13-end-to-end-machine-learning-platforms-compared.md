---
title: "End-to-end Machine Learning Platforms Compared"
excerpt: "A short and concise comparison into the industry’s leading Machine Learning Platforms."
date: 2019-07-13
header:
  overlay_image: "https://miro.medium.com/max/1400/1*MEOvALyLJrA9lJQvH0kBag.jpeg"
  overlay_filter: 0.5 # same as adding an opacity of 0.5 to a black background
  caption: "Photo credit: [**Martin Reisch**](https://unsplash.com)"
tags:
  - Machine Learning
  - Data Science
---

Any organisation involved with deploying machine learning models to production knows it comes with its share of business and technical challenges and will typically look to solve ‘some’ of those challenges by using a Machine Learning Platform complemented with some MLOps processes to increase maturity and governance in your team.

For organisations running multiple models in production and looking to adopt an ML platform they’ll typically either build an end-to-end ML platform in-house (Uber, Airbnb, Facebook Learner, Google TFX etc), or buy. In this article I am going to compare some ML Platforms which you can buy.

## When do you need an end-end ML Platform?

You should always answer another question first. “What problems are you trying to solve?”. If you can’t think of any existing problems, then there is no point in implementing new technology. However, if you are running into issues like, tracking what models or what versions are in production, increasing governance with your ML experiments, sharing notebooks as your data science team is growing or proactively monitoring data drift and/or feature drift, then chances are you going down or about to go down the path of implementing components of a ML platform.

To assist with this process I have performed a high-level comparison of what I’d like to think are key components of a ML platform.

## Comparison

The following comparison is considering out of the box functionality. E.g. not deploying an open-source solution to close a gap in functionality or building in-house.


|   | ![enter image description here](https://i.imgur.com/O71cG5j.png) | ![enter image description here](https://i.imgur.com/JtbBgjS.png) | ![enter image description here](https://i.imgur.com/ABbuUIH.png%5B/img%5D) | ![enter image description here](https://i.imgur.com/76KNmta.png)|
| --- | --- | --- | --- | --- |
|  **Features** | **AWS** | **GCP** | **Azure** | **Databricks** |
|  **Data pipeline** | [Data Pipeline](https://aws.amazon.com/datapipeline/) | [Dataflow](https://cloud.google.com/dataflow) | [Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/introduction) | Spark |
|  **Feature Store** | --- | --- | --- | --- |
|  **Model Monitoring** | [Model Monitor](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor.html) | --- | [Azure Monitor](https://docs.microsoft.com/en-us/azure/machine-learning/monitor-azure-machine-learning) | --- |
|  **Experiment Management** | [SageMaker Experiments](https://docs.aws.amazon.com/sagemaker/latest/dg/experiments.html#exp-mgmt-track) | --- | [Azure Machine Learning SDK](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-use-mlflow) | [MLFlow Tracking](https://www.mlflow.org/docs/latest/tracking.html) |
|  **Model versioning** | [Production Variants](https://aws.amazon.com/blogs/machine-learning/amazon-sagemaker-now-comes-with-new-capabilities-for-accelerating-machine-learning-experimentation/) | [Versions](https://cloud.google.com/ai-platform/training/docs/projects-models-versions-jobs) | [Model registration](https://docs.microsoft.com/en-us/azure/machine-learning/concept-model-management-and-deployment#register-package-and-deploy-models-from-anywhere) | [MLflow Model Registry](https://www.mlflow.org/docs/latest/model-registry.html) |
|  **A/B Testing** | [Sagemaker](https://aws.amazon.com/blogs/machine-learning/a-b-testing-ml-models-in-production-using-amazon-sagemaker/) | --- | [Controlled Rollout](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-deploy-azure-kubernetes-service#deploy-models-to-aks-using-controlled-rollout-preview) | --- |
|  **Model Serving** | [Sagemaker](https://docs.aws.amazon.com/sagemaker/latest/dg/deploy-model.html) | [AI Platform](https://cloud.google.com/ai-platform) | [Azure Machine Learning](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-deploy-and-where) | [MLFlow Model Serving](https://databricks.com/blog/2020/06/25/announcing-mlflow-model-serving-on-databricks.html) |
|  **AutoML** | [Autopilot](https://aws.amazon.com/sagemaker/autopilot/) | [Cloud AutoML](https://cloud.google.com/automl) | [AutomatedML](https://azure.microsoft.com/en-us/services/machine-learning/automatedml/) | --- |
|  **Notebooks** | [Sagemaker Notebooks](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi.html) | [AI Platform Notebooks](https://cloud.google.com/ai-platform-notebooks) | [Microsoft Azure Notebooks](https://notebooks.azure.com/) | [Notebooks](https://docs.databricks.com/notebooks/index.html) |



## Feature Store?

You might have noticed, not a single solution offers a feature store. Feature Stores are a fairly new component in the landscape and I suspect more organisations will be adding this to their ML platforms in the near future. Their are a few open-source solutions available, and even more research material on this amazing website. Just remember to focus on what problems you are aiming to solve, before investing time and money in a nonproblem.

    “What problems are you trying to solve?”

## Conclusion

To conclude this short article, I wanted to highlight some other products worth mentioning, but were not included in this comparison. DataRobot, IBM Watson Studio, H2o.ai, Data Bricks & Cloudera. I am going to endeavor to try keep this article up to date in the coming months as the machine learning landscape is changing rapidly. You don’t have to listen to me, just check out all the announcement’s on MLflow blog from Databricks at the Spark + AI Summit this year!

If you want your product added to this comparison please reach out to me on my LinkedIn.
