# Cors Routes Issue

Demo Jets Project to reproduce a cors routes issue:

* The issue occurs when the same route path that point to the different controllers with different with 2 different http verbs.

## To Reproduce

1. Deploy App 
2. Uncomment `post '/posts', to: 'api/v1/posts#create'` in [config/routes.rb](config/routes.rb)
3. Deploy again

Note: This issue seems to only occur in the us-east-2 region (ohio).

At some point, CloudFormation adjusted their behavior of nested cloudformation stacks with APIGW Method Resources.
