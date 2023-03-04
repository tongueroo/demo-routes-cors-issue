# Cors Routes Issue

Demo Jets Project to reproduce a cors routes issue:

* The issue occurs when the **same** route path that point to the **different** controllers with different with 2 **different http verbs**.

## Important Note

This issue seems to only occur in the us-east-2 region (ohio).

At some point, CloudFormation adjusted their behavior of nested cloudformation stacks where a duplicated resource of APIGW Method Resource for CORS would be allowed, now it's more strict and will error.

## To Reproduce

1. Deploy App 
2. Uncomment `post '/posts', to: 'api/v1/posts#create'` in [config/routes.rb](config/routes.rb)
3. Deploy again

## Error

The error looks something like this:

    Deploying CloudFormation stack with jets app!
    Waiting for stack to complete
    08:05:11PM UPDATE_IN_PROGRESS AWS::CloudFormation::Stack demo-dev User Initiated
    08:05:15PM UPDATE_IN_PROGRESS AWS::CloudFormation::Stack ApiGateway 
    08:05:15PM UPDATE_IN_PROGRESS AWS::IAM::Role IamRole 
    08:05:15PM UPDATE_COMPLETE AWS::CloudFormation::Stack ApiGateway 
    08:05:17PM UPDATE_IN_PROGRESS AWS::CloudFormation::Stack ApiResources1 
    08:05:18PM UPDATE_COMPLETE AWS::CloudFormation::Stack ApiResources1 
    08:05:29PM UPDATE_COMPLETE AWS::IAM::Role IamRole 
    08:05:31PM UPDATE_IN_PROGRESS AWS::CloudFormation::Stack PostsController 
    08:05:31PM UPDATE_IN_PROGRESS AWS::CloudFormation::Stack JetsPublicController 
    08:05:31PM UPDATE_IN_PROGRESS AWS::CloudFormation::Stack JetsPreheatJob 
    08:05:31PM UPDATE_IN_PROGRESS AWS::CloudFormation::Stack ApiV1PostsController 
    08:05:42PM UPDATE_FAILED AWS::CloudFormation::Stack ApiV1PostsController Embedded stack arn:aws:cloudformation:us-east-2:112233445566:stack/demo-dev-ApiV1PostsController-FIN5MLRE9O6R/b5387bc0-bab8-11ed-825e-0a120f70f250 was not successfully updated. Currently in UPDATE_ROLLBACK_IN_PROGRESS with reason: The following resource(s) failed to create: [PostsCorsApiMethod]. The following resource(s) failed to update: [ShowLambdaFunction, EditLambdaFunction, UpdateLambdaFunction, DeleteLambdaFunction, IndexLambdaFunction, CreateLambdaFunction, NewLambdaFunction]. 
    08:05:53PM UPDATE_FAILED AWS::CloudFormation::Stack JetsPreheatJob Resource update cancelled
    08:05:53PM UPDATE_FAILED AWS::CloudFormation::Stack JetsPublicController Resource update cancelled
    08:05:53PM UPDATE_FAILED AWS::CloudFormation::Stack PostsController Resource update cancelled
    08:05:54PM UPDATE_ROLLBACK_IN_PROGRESS AWS::CloudFormation::Stack demo-dev The following resource(s) failed to update: [ApiV1PostsController, JetsPublicController, PostsController, JetsPreheatJob]. 
    08:05:58PM UPDATE_IN_PROGRESS AWS::CloudFormation::Stack ApiGateway 
    08:05:58PM UPDATE_IN_PROGRESS AWS::IAM::Role IamRole 
    08:05:58PM UPDATE_COMPLETE AWS::CloudFormation::Stack ApiGateway 
    08:05:58PM UPDATE_IN_PROGRESS AWS::CloudFormation::Stack ApiResources1 
    08:05:59PM UPDATE_COMPLETE AWS::CloudFormation::Stack ApiResources1 
    08:06:12PM UPDATE_COMPLETE AWS::IAM::Role IamRole 
    08:06:13PM UPDATE_IN_PROGRESS AWS::CloudFormation::Stack JetsPreheatJob 
    08:06:13PM UPDATE_IN_PROGRESS AWS::CloudFormation::Stack ApiV1PostsController 
    08:06:13PM UPDATE_IN_PROGRESS AWS::CloudFormation::Stack PostsController 
    08:06:13PM UPDATE_IN_PROGRESS AWS::CloudFormation::Stack JetsPublicController 
    08:06:23PM UPDATE_COMPLETE AWS::CloudFormation::Stack ApiV1PostsController 
    08:06:34PM UPDATE_COMPLETE AWS::CloudFormation::Stack PostsController 
    08:06:35PM UPDATE_COMPLETE AWS::CloudFormation::Stack JetsPreheatJob 
    08:06:35PM UPDATE_COMPLETE AWS::CloudFormation::Stack JetsPublicController 
    08:06:36PM UPDATE_ROLLBACK_COMPLETE_CLEANUP_IN_PROGRESS AWS::CloudFormation::Stack demo-dev 
    08:06:47PM UPDATE_COMPLETE AWS::CloudFormation::Stack PostsController 
    08:06:47PM UPDATE_COMPLETE AWS::CloudFormation::Stack JetsPublicController 
    08:06:47PM UPDATE_COMPLETE AWS::CloudFormation::Stack JetsPreheatJob 
    08:06:47PM UPDATE_COMPLETE AWS::CloudFormation::Stack ApiV1PostsController 
    08:06:48PM UPDATE_COMPLETE AWS::CloudFormation::Stack ApiResources1 
    08:06:48PM UPDATE_COMPLETE AWS::CloudFormation::Stack ApiGateway 
    08:06:49PM UPDATE_ROLLBACK_COMPLETE AWS::CloudFormation::Stack demo-dev 
    Stack rolled back: UPDATE_ROLLBACK_COMPLETE
    Time took: 1m 43s
    The Jets application failed to deploy. Jets creates a few CloudFormation stacks to deploy your application.
    The logs above show the CloudFormation parent stack events and points to the stack with the error.
    Please go to the CloudFormation console and look for the specific stack with the error.
    The specific child stack usually shows more detailed information and can be used to resolve the issue.
    Example of checking the CloudFormation console: https://rubyonjets.com/docs/debugging/cloudformation/
