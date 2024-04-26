# Evaluating Large Language Models using LLM-as-a-Judge 

Evaluating large language models (LLM) is challenging due to their broad capabilities and the inadequacy of existing benchmarks in measuring human preferences. To address this, strong LLMs are used as judges to evaluate these models on more open-ended questions. The agreement between LLM judges and human preferences has been verified by introducing two benchmarks: Multi Turn (MT)-bench, a multi-turn question set, and Chatbot Arena, a crowdsourced battle platform. The results reveal that strong LLM judges can match both controlled and crowdsourced human preferences well, achieving over 80% agreement, the same level of agreement between humans This makes LLM-as-a-judge a scalable and explainable way to approximate human preferences, which are otherwise very expensive to obtain.

> ℹ️  **Note:** The evaluation steps in this lab are based on the paper  [Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena](https://arxiv.org/pdf/2306.05685.pdf).

This lab addresses this challenge by providing a practical solution for evaluating LLMs using LLM-as-a-Judge with Amazon Bedrock. This is relevant for developers and researchers working on evaluating LLM based applications. In the notebook you are guided using MT-Bench questions to generate test answers and evaluate them with a single-answer grading using the Bedrock API, Python and Langchain. The notebook consists of the following chapters: 

1) Set-up of the environment
2) Load MT-Bench questions
3) Generate test answers from LLM which should be evaluated
4) Evaluate answers with strong LLM-as-a-judge
5) Generate explanation for average rating score


## Getting started

### Choose a notebook environment

This lab is presented as a **Python notebook**, which you can run from the environment of your choice:

- [SageMaker Studio](https://aws.amazon.com/sagemaker/studio/) is a web-based integrated development environment (IDE) for machine learning. To get started quickly, refer to the [instructions for domain quick setup](https://docs.aws.amazon.com/sagemaker/latest/dg/onboard-quick-start.html).
- [SageMaker Notebook Instance](https://docs.aws.amazon.com/sagemaker/latest/dg/howitworks-create-ws.html) is a machine learning (ML) compute instance running the Jupyter Notebook App.
- To use your existing (local or other) notebook environment, make sure it has [credentials for calling AWS](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html).


### Enable AWS IAM permissions for Bedrock

The AWS identity you assume from your notebook environment (which is the [*Studio/notebook Execution Role*](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html) from SageMaker, or can be a role or IAM User for self-managed notebooks), must have sufficient [AWS IAM permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) to call the Amazon Bedrock service.

To grant Bedrock access to your identity:

- Open the [AWS IAM Console](https://us-east-1.console.aws.amazon.com/iam/home?#)
- Find your [Role](https://us-east-1.console.aws.amazon.com/iamv2/home?#/roles) (if using SageMaker or otherwise assuming an IAM Role), or else [User](https://us-east-1.console.aws.amazon.com/iamv2/home?#/users)
- Select *Add Permissions > Create Inline Policy* to attach new inline permissions, open the *JSON* editor and paste in the below example policy:

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Sid": "AllowInference",
        "Effect": "Allow",
        "Action": [
            "bedrock:InvokeModel"
        ],
        "Resource": "arn:aws:bedrock:*::foundation-model/*"
    }
}
```

> ℹ️  **Note:** With Amazon SageMaker, your notebook execution role is typically be *separate* from the user or role that you log in to the AWS Console with. If you want to explore the AWS Console for Amazon Bedrock, you need to grant permissions to your Console user/role too. You can run the notebooks anywhere as long as you have access to the AWS Bedrock service and have appropriate credentials

For more information on the fine-grained action and resource permissions in Bedrock, check out the Bedrock Developer Guide.


### Clone and use the notebooks

> ℹ️ **Note:** In SageMaker Studio, you can open a "System Terminal" to run these commands by clicking *File > New > Terminal*

Once your notebook environment is set up, clone this workshop repository into it.

```sh
sudo yum install -y unzip
git clone git@github.com:aws-samples/evaluating-large-language-models-using-llm-as-a-judge.git
cd evaluating-large-language-models-using-llm-as-a-judge
```

You're now ready to explore the lab notebook! You will be guided through connection the notebook to Amazon Bedrock for large language model access.


## Contributing

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License
This library is licensed under the MIT-0 License. See the [LICENSE](LICENSE) file.

