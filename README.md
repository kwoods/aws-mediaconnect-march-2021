# Issues with AWS MediaConnect CloudFormation Resources

List of issues identified when working with AWS MediaConnect resource using CloudFormation

## Documentation Improvement Opportunities 

### AWS::MediaConnect::Flow
https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-mediaconnect-flow.html

* Return Value for `IngestIp` not available
> Requested attribute IngestIp does not exist in schema for AWS::MediaConnect::Flow

* Return Value for `SourceArn` not available
> Requested attribute SourceArn does not exist in schema for AWS::MediaConnect::Flow

### AWS::MediaConnect::FlowOutput  
https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-mediaconnect-flowoutput.html

* Return Value for `OutputArn` not available
> Unable to retrieve OutputArn attribute for AWS::MediaConnect::FlowOutput, with error message Unable to marshall request to JSON: Parameter 'flowArn' must not be null

* Flow Output (property)[https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-mediaconnect-flowoutput.html#cfn-mediaconnect-flowoutput-cidrallowlist] `CidrAllowList` is limited to 3 ranges but the documentation does not indicate a limit.
  - Attempting to add more than 3 CIDR ranges results in: `Error occurred during operation 'AWS::MediaConnect::FlowOutput'.`

### AWS::MediaConnect::FlowVpcInterface
https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-mediaconnect-flowvpcinterface.html

* Flow VPC Interface (property)[https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-mediaconnect-flowvpcinterface.html] `SubnetId` description appears to indicate multiple values are possible:
> The subnet IDs that you want to use for your VPC interface.
  - This property does not accept a List of String values
  - Using a SubnetId in a different AZ than the flow results in: `Error occurred during operation 'AWS::MediaConnect::FlowVpcInterface'.`  Would be more helpful to indicate wrong AZ.  Documentation does state subnet must be in the same AZ but the stack failure could be more indicative.

* Return Value for `Ref` does not provide Interface Name
  - Example value returned from `!Ref`:  `arn:aws:mediaconnect:us-east-1:0123456789:flow:1-ABCD1234-0123456:flow1|flow1-interface1`
  - Unable to use for the `VpcInterfaceAttachment` property in `AWS::MediaConnect::FlowOutput`

* Return Value for `NetworkInterfaceIds` not available
> Template format error: Every Value member must be a string.



## Other Issues

* Flow Source (property)[https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-mediaconnect-flow-source.html#cfn-mediaconnect-flow-source-whitelistcidr] `WhitelistCidr` does not support multiple ranges
* Stack Tags do not propagate down to Flow resource created 

* Flow Console - inside flow, if you click details w/o first selecting an output, you will see the details of the last outputs's details you viewed.  Details button should not enable without selecting an output first.

* Unable to create a VPC type Flow Source due to circular dependance between the following two resources:
  - AWS::MediaConnect::Flow
  - AWS::MediaConnect::FlowVpcInterface
  Requires creating the flow as a Standard type source and then either in Console or CLI switching the flow source to use the created VPC interface: e.g. `aws mediaconnect update-flow-source`