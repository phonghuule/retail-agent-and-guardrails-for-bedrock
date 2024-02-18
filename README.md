# Build a customer service bot with agents for Amazon Bedrock and apply Guardrails for responsible AI policies

This lab is provided as part of **[AWS Innovate AI/ML and Data Edition](https://aws.amazon.com/events/aws-innovate/apj/aiml-data/)**, it has been adapted from the [blog post](https://aws.amazon.com/blogs/machine-learning/build-a-foundation-model-fm-powered-customer-service-bot-with-agents-for-amazon-bedrock/).

ℹ️ You will run this lab in your own AWS account. Please follow directions at the end of the lab to remove resources to avoid future costs.

ℹ️ Please let us know what you thought of this session and how we can improve the experience for you in the future by completing [the survey](#survey) at the end of the lab.
Participants who complete the surveys from AWS Innovate Online Conference will receive a gift code for USD25 in AWS credits <sup> 1, 2 & 3 </sup>.

## **Overview**
In this lab, you will learn how to use [agents for Amazon Bedrock] to create fully managed agents in a few clicks. You can follow step-by-step guide with building blocks to create a customer service bot. We use a text generation model (Anthropic Claude V2) and agents for Amazon Bedrock for this solution. We provide an AWS CloudFormation template to provision the resources needed for building this solution. 

## **Architecture**
The following diagram depicts a high-level architecture of this solution.

![Architecture](/images/arch-diagram.png)

1. You can create an agent with Amazon Bedrock-supported FMs such as Anthropic Claude V2.
2. Attach API schema, residing in an Amazon Simple Storage Service (Amazon S3) bucket, and a Lambda function containing the business logic to the agent. (Note: This is a one-time setup step.)
3. The agent uses customer requests to create a prompt using the ReAct framework. It, then, uses the API schema to invoke corresponding code in the Lambda function.
4. You can perform a variety of tasks, including sending email notifications, writing to databases, and triggering application APIs in the Lambda functions.

## Setup
### Request Access To Model

### Deploy Cloud Formation



## Lab
### Fine-tuning an LLM
In this section we will use Amazon SageMaker to run an LLM fine-tuning job using the Hugging Face Optimum Neuron library and an AWS EC2 trn1.2xlarge instance (featuring Trainium accelerators).

ℹ️  Navigate to the notebook folder /01_finetuning/ and launch the Finetune-TinyLlama-1.1B.ipynb notebook.

Please refer to the above-mentioned Jupyter notebook and run each of the notebook cells in order to complete this lab module. The following content is provided as reference material which may help you as you work through the notebook.

#### AWS Trainium
[AWS Trainium](https://aws.amazon.com/machine-learning/trainium/) is the second-generation machine learning (ML) accelerator that AWS purpose built for deep learning training of 100B+ parameter models. Each Amazon Elastic Compute Cloud (EC2) Trn1 instance deploys up to 16 AWS Trainium accelerators to deliver a high-performance, low-cost solution for deep learning (DL) training in the cloud. Although the use of deep learning is accelerating, many development teams are limited by fixed budgets, which puts a cap on the scope and frequency of training needed to improve their models and applications. Trainium based EC2 Trn1 instances solve this challenge by delivering faster time to train while offering up to 50% cost-to-train savings over comparable Amazon EC2 instances.

In this lab you will work with a single trn1.2xlarge instance containing a single Trainium accelerator with 2 NeuronCores. While the trn1.2xlarge instance is useful for fine-tuning, AWS also offers larger trn1.32xlarge and trn1n.32xlarge instance types that each contain 16 Trainium accelerators (32 NeuronCores) and are capable of large-scale distributed training.

#### Hugging Face Optimum Neuron
[Optimum Neuron](https://huggingface.co/docs/optimum-neuron) is a Python library providing the interface between Hugging Face Transformers and the purpose-built AWS ML accelerators - Trainium and Inferentia. Optimum Neuron provides a set of tools enabling easy model loading, training, and inference on single and multi-accelerator settings for different downstream tasks. The goal of this library is to provide an easy-to-use mechanism for users to leverage the most popular Hugging Face models on Trainium and Inferentia, using familiar Transformers concepts such as the Trainer API. 

### Deploying a fine-tuned LLM for inference
In this section we will use Amazon SageMaker and Amazon EC２ Inf２ instance to deploy the model fine-tuned in the previous section. Amazon SageMaker deployment provides fully managed options for deploying our models using Real Time or Batch modes. AWS Inferentia gives best cost per inference.

ℹ️   Navigate to the notebook folder /02_inference/ and launch Inference-TinyLlama-1.1B.ipynb notebook.

Please refer to the above-mentioned Jupyter notebook and run each of the notebook cells in order to complete this lab module. The following content is provided as reference material which may help you as you work through the notebook.

#### Inferentia2
[AWS Inferentia2](https://aws.amazon.com/machine-learning/inferentia) is the second generation purpose built Machine Learning inference accelerator from AWS. Amazon EC2 Inf2 instances are powered by AWS Inferentia2. Inf2 instances raise the performance of Inf1 by delivering 3x higher compute performance, 4x larger total accelerator memory, up to 4x higher throughput, and up to 10x lower latency. Inf2 instances are the first inference-optimized instances in Amazon EC2 to support scale-out distributed inference with ultra-high-speed connectivity between accelerators. You can now efficiently and cost-effectively deploy models with hundreds of billions of parameters across multiple accelerators on Inf2 instances.

In this lab you will work with a single inf2.xlarge instance containing a single Inferentia2 accelerator with 2 NeuronCores. While the inf2.xlarge instance is useful for most cost-optimized inference, AWS also offers larger inf2.48xlarge and inf2.24xlarge instance types that each contain 12 and 6 Inferentia2 accelerators (24 and 12 NeuronCores) and are capable of more performance optimized inference. 

## Summary
Now that you have completed the session, you should have an understanding of:
- Using Optimum Neuron to streamline the fine-tuning of LLMs with AWS Trainium
- Launching an LLM fine-tuning job using SageMaker Training and storing your fine-tuned model in Amazon S3
- Preparing your model for deployment on AWS Inferentia using transformers-neuronx
- Deploying your model on a SageMaker Hosting endpoint using DJL Serving

## **Clean Up**
1. Return to the AWS Console, and search for Amazon SageMaker to launch the SageMaker console. In the left-hand menu, choose **Training -> Training jobs**. If any training jobs show a status of Running, please select the jobs and choose **Action -> Stop**to stop the jobs. Repeat this step for any additional regions that you used during the lab.

2. Choose **Inference -> Inference endpoints** from the left-hand menu. If any endpoints appear in the list (they should have tinyllama in the name), please select and delete the endpoints using the Actions menu. Repeat this step for any additional regions that you used during the workshop.

3. From the left-hand menu in the SageMaker console, choose **Domains**. Select the domain that you created for this workshop (likely starts with **QuickSetupDomain-**). On the **User Profiles** screen, select the user profile to be deleted (ex: default-20231211t230805). On the Apps screen, select the **'default'** app and scroll to the right to expose an Action menu. From the Action menu, choose Delete. When the confirmation appears, choose Yes, delete app and then type 'delete' in the textbox to to delete the user profile.

4. Once the SageMaker Studio user profile has been deleted, return to the SageMaker console and choose **Domains** from the left-hand menu. Select the radio button next to the domain you created for this lab, and then click Edit. At the top of the **General Settings** screen you should see a **Delete domain** button. Click this button and follow the prompts to remove the SageMaker Studio domain from your account.

5. At the top of the AWS Console, search for S3 to open the S3 console. In the list of S3 buckets, look for buckets that start with **sagemaker-**. View the contents of these buckets and delete any artifacts that you do not want to keep.

## Survey
Let us know what you thought of this session and how we can improve the presentation experience for you in the future by completing [this event session poll](https://amazonmr.au1.qualtrics.com/jfe/form/SV_5BWPHDlxVcsRbo2?Session=HOL02). 
Participants who complete the surveys from AWS Innovate Online Conference will receive a gift code for USD25 in AWS credits <sup> 1, 2 & 3 </sup>. AWS credits will be sent via email by November 30, 2023.
Note: Only registrants of AWS Innovate Online Conference who complete the surveys will receive a gift code for USD25 in AWS credits via email.

<sup>1</sup>AWS Promotional Credits Terms and conditions apply: https://aws.amazon.com/awscredits/

<sup>2</sup>Limited to 1 x USD25 AWS credits per participant.

<sup>3</sup>Participants will be required to provide their business email addresses to receive the gift code for AWS credits.