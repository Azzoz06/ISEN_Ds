# Predict loan eligibility using IBM Watson Studio
Build a predictive model to automate the process of targeting the right applicants.

*Inspired By Hissah AlMuneef | Published January 18, 2019*

---

Loans are the core business of loan companies. The main profit comes directly from the loan’s interest. The loan companies grant a loan after an intensive process of verification and validation. However, they still don’t have assurance if the applicant is able to repay the loan with no difficulties.

In this tutorial, we’ll build a predictive model to predict if an applicant is able to repay the lending company or not. We will prepare the data using Jupyter Notebook and then build the model using SPSS Modeler.
### Learning objectives

After completing this tutorial, you’ll understand how to:

    Add and prepare your data
    Build a machine learning model
    Save the model

### Prerequisites

In order to complete this tutorial, you will need the following:

    + IBM Cloud account.
    + Object Storage Service.
    + Watson Studio Service.
    + Machine Learning Service.

### Estimated time

The overall time of reading and following this tutorial is approximately one hour.
### Steps
#### Dataset

The dataset is from [Analytics Vidhya](https://datahack.analyticsvidhya.com/contest/practice-problem-loan-prediction-iii/#data_dictionary) and from the Git folder [LabSources](LabSources/)

The format of the data:
  + **Variable** Description
  + **Loan_ID** Unique Loan ID
  + **Gender** Male/ Female
  + **Married** Applicant married (Y/N)
  + **Dependents** Number of dependents
  + **Education** Applicant Education (Graduate/ Under Graduate)
  + **Self_Employed** Self employed (Y/N)
  + **ApplicantIncome** Applicant income
  + **CoapplicantIncome** Coapplicant income
  + **LoanAmount** Loan amount in thousands
  + **Loan_Amount_Term** Term of loan in months
  + **Credit_History** Credit history meets guidelines
  + **Property_Area** Urban/ Semi Urban/ Rural
  + **Loan_Status** Loan approved (Y/N)

### Step 1. Create a project in Watson Studio

From Watson Studio main page, click on New project. Choose Complete to get the full functionalities. Once you enter your project name, click on Create.

### Step 2. Upload the dataset to Watson Studio

Open Find and add data on the right-side panel, drag and drop the dataset (.csv file) from your computer to that area.

### Step 3. Create SPSS modeler flow
  1. On the same Assets page, click `Add to project` to Modeler flows.
  ![](assets/markdown-img-paste-20190218142838839.png)
  2. Select Modeler Flow
  3. Under the ‘New’ tab, name your modeler ‘Loan Eligibility Predictive model’.
  > Leave the default options to **Modeler Flow** and **IBM SPSS Modeler**  

  4. Click Create.

### Step 4. Add and prepare data

  1. Add data to the canvas using the Data Asset node.![](assets/markdown-img-paste-20190218143123876.png)
  2. Double click on the node and click Change Data Asset to open the Asset Browser. ![](assets/markdown-img-paste-20190218143218739.png)
  3. Select `loan_train.csv` then click OK and Save.

  Let’s look into the summary statistics of our data using the Data Audit node.

  + Drag and drop the Data Audit node, and connect it with the Data Asset node. After running the node you can see your audit report on right side panel.![](assets/markdown-img-paste-20190218143412932.png)  

  ![](assets/markdown-img-paste-20190218143455764.png)

  We can see that some columns have missing values. Let’s remove the rows that have null values using the Select node.

1. Drag and drop the Select node, connect it with the Data Asset node and right click on it and open the node.
2. Select discard mode and provide the below condition to remove rows with null values.

`(@NULL(Gender) or @NULL(Married) or @NULL(Dependents) or @NULL(Self_Employed) or @NULL(LoanAmount) or @NULL(Loan_Amount_Term) or @NULL(Credit_History))
`

![](assets/markdown-img-paste-20190218143702644.png)

Now our data is clean, and we can proceed with building the model.
### Step 5. Configure variables type

1. Drag and Drop the Type node to configure variables type, from Field Operations palette.![](assets/markdown-img-paste-20190218143752170.png)
2. Double click the node or right click to open it.
3. Choose Configure Types to read the metadata.
4. Change the Role from the drop down menu of [Loan_Status] from input to `target`.
5. Change the Role drop down menu of [LoanID] from none to Record ID.
![](assets/markdown-img-paste-2019021814394272.png)
6. Click Save.

### Step 6. Build a machine learning model

The model predicts the loan eligibility of two classes (Either Y:Yes or N:No). Thus, the choice of algorithms fell into Bayesian networks since it’s known to give good results for predicting classification problems.

1. Split Data into training and testing sets using the Partition node, from Field Operations palette.

2. Double click the Partition node to customize the partition size into 80:20, change the ratio in the Training Partition to 80 and Testing Partition to 20 and click Save.
![](assets/markdown-img-paste-2019021814412521.png)

3. Drag and drop the Bayes Net node from the Modeling Palette.

4. Double click the node to change the settings. Check Use custom field roles to assign Loan_Status as the target and all the remaining attributes as input except Partition and Loan_ID. When you finish, click Save.![](assets/markdown-img-paste-20190218144656410.png)

5. Run your Bayesian Network node, then you’ll see your model in an orange colored node.![](assets/markdown-img-paste-20190218144954455.png)

### Step 7. View the model

1. Right click on the orange colored node, then click on View Model.
2. Now you can see the Network Graph and other model information here.![](assets/markdown-img-paste-20190218145031847.png)

### Step 8. Evaluate the performance of the model

1. Drag and drop the Analysis node from the Output section, and connect it with the model.![](assets/markdown-img-paste-2019021814521406.png)
2. After running the node, you can see your analysis report on the right side panel.

The analysis report shows we have achieved 72.21% accuracy on our test data set with this model. At the end, you can build more models within the same canvas until you get the result you want.
### Step 9. Save the Model

Right-click on the Bayes Net node and select Save branch as a model. Enter a name for the model. ![](assets/markdown-img-paste-20190218145419331.png)A machine learning service should be added automatically if you already created one if not you can create one from the save dialogue. Click on Save.

In the Asset page under Watson Machine Learning models you can access your saved model, where you can deploy it and test it later.
![](assets/markdown-img-paste-20190218145717440.png)

**For training purpose, deploy and test the model to see how it scores out of sample data**

### Summary

In this tutorial, you learned how to create a complete predictive model, from importing the data, preparing the data, to training and saving the model. You also learned how to use SPSS Modeler and export the model to Watson Machine Learning models.
