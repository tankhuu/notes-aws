# CDK Concepts

## Constructs

- Basic building blocks of AWS CDK apps.
- Represents a Cloud Component

### Low-level constructs

which we call CFN Resources (or L1, short for "level 1") or Cfn resources. These constructs directly represent all resources available in AWS CloudFormation.

[AWS CloudFormation Resource Specification](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-resource-specification.html)

Ex: CfnBucket -> AWS::S3::Bucket

### Level 2 constructs

Also represent AWS resources, but with a higher-level, intent-based API. They provide similar functionality, but provide the defaults, boilerplate, and glue logic you'd be writing yourself with a CFN Resource construct.

### AWS Construct Library

Higher-level constructs, which we call patterns.
These constructs are designed to help you complete common tasks in AWS, often involving multiple kinds of resources.

[API Reference](https://docs.aws.amazon.com/cdk/api/latest/docs/aws-construct-library.html)

## Composition

Composition is the key pattern for defining higher-level abstractions through constructs. A high-level construct can be composed from any number of lower-level constructs, and in turn, those could be composed from even lower-level constructs, which eventually are composed from AWS resources.

## Initialization

Constructs are implemented in classes that extend the Construct base class. You define a construct by instantiating the class. All constructs take three parameters when they are initialized:

### Scope

The construct within which this construct is defined. You should usually pass this for the scope, because it represents the current scope in which you are defining the construct.

### Id

An identifier that must be unique within this scope. The identifier serves as a namespace for everything that's defined within the current construct and is used to allocate unique identities such as resource names and AWS CloudFormation logical IDs.

### Props

A set of properties or keyword arguments, depending upon the language, that define the construct's initial configuration. In most cases, constructs provide sensible defaults, and if all props elements are optional, you can leave out the props parameter completely.

Identifiers need only be unique within a scope. This lets you instantiate and reuse constructs without concern for the constructs and identifiers they might contain, and enables composing constructs into higher level abstractions. In addition, scopes make it possible to refer to groups of constructs all at once, for example for tagging or for specifying where the constructs will be deployed.

## Apps and stacks

CDK application an app, which is represented by the AWS CDK class App.

```
from aws_cdk.core import App, Stack
from aws_cdk import aws_s3 as s3

class HelloCdkStack(core.Stack):

    def __init__(self, scope: core.Construct, id: str, **kwargs) -> None:
        super().__init__(scope, id, **kwargs)

        s3.Bucket(self, "MyFirstBucket", versioned=True)

app = core.App()
HelloCdkStack(app, "HelloCdkStack")
```

Stacks in AWS CDK apps extend the Stack base class.
This is a common pattern when creating a stack within your AWS CDK app: extend the Stack class, define a constructor that accepts scope, id, and props, and invoke the base class constructor via super with the received scope, id, and props, as shown in the following example.

## The construct tree

As we've already seen, in AWS CDK apps, you define constructs "inside" other constructs using the scope argument passed to every construct. In this way, an AWS CDK app defines a hierarchy of constructs known as the construct tree.

The root of this tree is your app—that is, an instance of the App class. Within the app, you instantiate one or more stacks. Within stacks, you instantiate either AWS CloudFormation resources or higher-level constructs, which may themselves instantiate resources or other constructs, and so on down the tree.

Constructs are always explicitly defined within the scope of another construct, so there is never any doubt about the relationships between constructs. Almost always, you should pass this (in Python, self) as the scope, indicating that the new construct is a child of the current construct. The intended pattern is that you derive your construct from core.Construct, then instantiate the constructs it uses in its constructor.

Passing the scope explicitly allows each construct to add itself to the tree, with this behavior entirely contained within the Construct base class. It works the same way in every language supported by the AWS CDK and does not require introspection or other "magic."

Important
Technically, it's possible to pass some scope other than this when instantiating a construct, which allows you to add constructs anywhere in the tree, even in another stack entirely. For example, you could write a mixin-style function that adds constructs to a scope passed in as an argument. The practical difficulty here is that you can't easily ensure that the IDs you choose for your constructs are unique within someone else's scope. The practice also makes your code more difficult to understand, maintain, and reuse. It is virtually always better to find a way to express your intent without resorting to abusing the scope argument.

The AWS CDK uses the IDs of all constructs in the path from the tree's root to each child construct to generate the unique IDs required by AWS CloudFormation. This approach means that construct IDs need be unique only within their scope, rather than within the entire stack as in native AWS CloudFormation. It does, however, mean that if you move a construct to a different scope, its generated stack-unique ID will change, and AWS CloudFormation will no longer consider it the same resource.

The construct tree is separate from the constructs you define in your AWS CDK code, but it is accessible through any construct's node attribute, which is a reference to the node that represents that construct in the tree. Each node is a ConstructNode instance, the attributes of which provide access to the tree's root and to the node's parent scopes and children.

node.children – The direct children of the construct.

node.id – The identifier of the construct within its scope.

node.path – The full path of the construct including the IDs of all of its parents.

node.root – The root of the construct tree (the app).

node.scope – The scope (parent) of the construct, or undefined if the node is the root.

node.scopes – All parents of the construct, up to the root.

node.uniqueId – The unique alphanumeric identifier for this construct within the tree (by default, generated from node.path and a hash).

The construct tree defines an implicit order in which constructs are synthesized to resources in the final AWS CloudFormation template. Where one resource must be created before another, AWS CloudFormation or the AWS Construct Library will generally infer the dependency and make sure the resources are created in the right order. You can also add an explicit dependency between two nodes using node.addDependency(); see [Dependencies](https://docs.aws.amazon.com/cdk/api/latest/docs/core-readme.html#dependencies) in the AWS CDK API Reference.

The AWS CDK provides a simple way to visit every node in the construct tree and perform an operation on each one. See [Aspects](https://docs.aws.amazon.com/cdk/latest/guide/aspects.html)
