# Optimizing GenAI Applications for Speed  

Generative AI applications are transforming how we do business today, creating new, engaging ways for customers to engage with applications. However, these new LLM models require massive amounts of compute to run, and unoptimized applications can run quite slowly, leading users to become frustrated. Creating a positive user experience is critical to the adoption of these tools, so minimising the response time of your LLM API calls is a must. The techniques shared in this article demonstrate how applications can be sped up by up to 100x their original speed through clever prompt engineering and a small amount of code!  
  
Previous work has identified the core principles for reducing LLM response times. This article expands upon these, by providing practical examples coupled with working code, to help you accelerate your own applications and delight customers. This article is primarily intended for software developers, data scientists and application developers, though any business stakeholder managing GenAI applications should read on to learn new ideas for improving their customer experience.  
  
## Understanding the drivers of long response times  
The response time of an LLM can vary based on four primary factors:   
1. The model used.  
2. The number of tokens in the prompt.  
3. The number of tokens generated.  
4. The overall load on the deployment & system.   
  
You can imagine the model as a person typing on a keyboard, where each token is generated one after another. The speed of the person (the model used) and the amount they need to type (the number of generation tokens) tend to be the largest contributor to long response times.  
  
*Figure 1 - The response generation step typically dominates the overall response time. Not to scale.*  
  
## Techniques for improving LLM response times  
  
The below table contains a range of recommendations that can be implemented to improve the response times of your Generative AI application. Where applicable, sample code is included, to allow you to see these benefits for yourself, and copy the relevant code or prompts into your application.  
  
| Best Practice | Intuition | GitHub | Potential Speed up of application |  
| --- | --- | --- | --- |  
| 1. Generation Token Compression | Prompt the LLM to return the shortest response possible. A few simple phrases in your prompt can speed up your application. Few-shot prompting can also be used to ensure the response includes all the key information. | [Link](https://github.com/Azure/The-LLM-Latency-Guidebook-Optimizing-Response-Times-for-GenAI-Applications/blob/main/notebooks-with-techniques/generation-token-compression/generation-token-compression.ipynb) | Up to 2-3x or more<br>20s → 8s |  
| 2. Avoid using LLMs to output large amounts of predetermined text | Rather than rewriting documents, use the LLM to identify which parts of the text need to be edited, and use code to make the edits. For RAG, use code to simply append documents to the LLM response. | [Link](https://github.com/Azure/The-LLM-Latency-Guidebook-Optimizing-Response-Times-for-GenAI-Applications/blob/main/notebooks-with-techniques/avoid-rewriting-documents/avoid-rewriting-documents.ipynb) | Up to 16x or more<br>310s → 20s |  
| 3. Implement semantic caching | By caching responses, LLM responses can be reused, rather than calling Azure OpenAI, saving cost and time. The input does not need an exact match- for example “How can I sign up for Azure” and “I want to sign up for Azure” will return the same cached result. | [Link](https://github.com/Azure/The-LLM-Latency-Guidebook-Optimizing-Response-Times-for-GenAI-Applications/blob/main/notebooks-with-techniques/semantic-caching/semantic-caching.ipynb) | Up to 14x or more<br>19s → 1.3s |  
| 4. Parallelize requests | Many use cases (such as document processing, classification etc.) can be parallelized. | [Link](https://github.com/Azure/The-LLM-Latency-Guidebook-Optimizing-Response-Times-for-GenAI-Applications/blob/main/notebooks-with-techniques/parallelization/parallelization.ipynb) | Up to 72x or more<br>180s → 2.5s |  
| 5. Use GPT-3.5 over GPT-4 where possible | GPT-3.5 has a much faster token generation speed. Certain use cases require the more advanced reasoning capabilities of GPT-4, however sometimes few-shot prompting or finetuning may enable GPT-3.5 to perform the same tasks. Generally only recommended for advanced users, after attempting other optimizations first. | [Link](https://github.com/Azure/The-LLM-Latency-Guidebook-Optimizing-Response-Times-for-GenAI-Applications/blob/main/notebooks-with-techniques/use-models-with-faster-time-between-tokens/use-models-with-faster-time-between-tokens.ipynb) | Up to 4x<br>17s → 5s |  
| 6. Leverage translation services for certain languages | Certain languages have not been optimised, leading to long response times. Generate the output in English and leverage another model or API for the translation step. | [Link](https://github.com/Azure/The-LLM-Latency-Guidebook-Optimizing-Response-Times-for-GenAI-Applications/blob/main/notebooks-with-techniques/multilingual-optimization/multilingual-optimization.ipynb) | Up to 3x<br>53s → 16s |  
| 7. Co-locate cloud resources | Ensure model is deployed close your users. Ensure Azure AI Search and Azure OpenAI are as closely located as possible (in the same region, firewall, vNet etc.). | NA | 1-2x |  
| 8. Load balancing | Having an additional endpoint for handling overflow capacity (for example, a PTU overflowing to a Pay-as-you-Go endpoint) can save latency by avoiding queuing when retrying requests. | [Link](https://github.com/Azure/The-LLM-Latency-Guidebook-Optimizing-Response-Times-for-GenAI-Applications/blob/main/notebooks-with-techniques/load-balancing/load-balancing.ipynb) | Up to 2x<br>58s → 31s |  

## Putting it into practice through case studies  
  
This section includes an overview of case studies that represent typical GenAI applications—perhaps one is similar to yours! The linked code repositories show the original speed of the application, and then walk you step-by-step through the process of implementing different combinations of the techniques in this document. Implementing these recommendations achieved an improvement in the response time ranging from 6.8-102x!  


<table>  
  <tr style="border-bottom: 2px solid black;">  
    <th>Case Study</th>  
    <th>Techniques applied</th>  
    <th>Cumulative speed improvement</th>  
    <th>GitHub</th>  
  </tr>  
  <tr>  
    <th rowspan="4" style="border-right: 1px solid #ddd; border-bottom: 1px solid black;">Document processing<br>Rewrite a document to correct spelling errors, grammar, and comply with an organization’s style guide.</th>  
    <td>1. Base case</td>  
    <td>1x (315s)</td>  
    <td rowspan="4" style="border-left: 1px solid #ddd; border-bottom: 2px solid black;"><a href="https://github.com/Azure/The-LLM-Latency-Guidebook-Optimizing-Response-Times-for-GenAI-Applications/blob/main/case-studies/document-processing/document-processing.ipynb">Link</a></td>  
  </tr>  
  <tr>  
    <td>2. Avoid rewriting documents</td>  
    <td>8.3x (38s)</td>  
  </tr>  
  <tr>  
    <td>3. Generation token compression</td>  
    <td>15.8x (20s)</td>  
  </tr>  
  <tr style="border-bottom: 2px solid black;">  
    <td>4. Parallelization</td>  
    <td>105x (3s)</td>  
  </tr>  
  <tr>  
    <th rowspan="3" style="border-right: 1px solid #ddd;">Retrieval Augmented Generation (RAG)<br>Help a user troubleshoot a product which is not working.</th>  
    <td>1. Base case</td>  
    <td>1x (23s)</td>  
    <td rowspan="3" style="border-left: 1px solid #ddd; border-bottom: 2px solid black;"><a href="https://github.com/Azure/The-LLM-Latency-Guidebook-Optimizing-Response-Times-for-GenAI-Applications/blob/main/case-studies/retrieval-augmented-generation/retrieval-augmented-generation.ipynb">Link</a></td>  
  </tr>  
  <tr>  
    <td>2. Generation token compression</td>  
    <td>2.3x (9.8s)</td>  
  </tr>  
  <tr style="border-bottom: 2px solid black;">  
    <td>3. Avoid rewriting documents</td>  
    <td>6.8x (3.4s)</td>  
  </tr>  
  <tr>  
    <th rowspan="2" style="border-right: 1px solid #ddd;">Retrieval Augmented Generation (RAG)<br>Provide general product information.</th>  
    <td>1. Base case</td>  
    <td>1x (17s)</td>  
    <td rowspan="2" style="border-left: 1px solid #ddd; border-bottom: 2px solid black;"><a href="https://github.com/Azure/The-LLM-Latency-Guidebook-Optimizing-Response-Times-for-GenAI-Applications/blob/main/case-studies/retrieval-augmented-generation/retrieval-augmented-generation.ipynb">Link</a></td>  
  </tr>  
  <tr style="border-bottom: 2px solid black;">  
    <td>2. Semantic caching</td>  
    <td>17x (1s)</td>  
  </tr>  
</table>  

## Conclusion  
  
With Generative AI transforming how people interact with applications, minimising response times is essential. If you’re interested in improving your GenAI application’s performance, select a few of these recommendations, clone the repository, and implement them in your application’s next release!  
  
_Disclaimer: The results depicted are merely illustrative, emphasizing the potential benefits of these techniques. They are not all-encompassing and are based on a single test. Response times may differ with each run, thus the main goal is to demonstrate relative improvement. The tests are performed using the powerful, but slower, GPT-4 32k model, with a focus on improving response times. The effectiveness of techniques like error correction through document rewriting varies depending on the input; a document with many errors might take longer to correct than to rewrite entirely. Therefore, these techniques should be tailored to your application._  


## Contributing

**Thanks to the following key contributors:** Luca Stamatescu, Priya Kedia, Julian Lee, Manoranjan Rajguru, Shikha Agrawal, Michael Tremeer  
  

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft 
trademarks or logos is subject to and must follow 
[Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.
