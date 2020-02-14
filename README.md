
English | [简体中文](README-CN.md)

<p align="center">
<a href=" https://www.alibabacloud.com"><img src="https://www.capitalonline.net/templets/default/icon/logo_header.png"></a>
</p>

<h1 align="center">CapitalOnline Cloud SDK for Go</h1>

The project is aim to build the golang sdk for [CapitalOnline](https://www.capitalonline.net) Cloud Platform. It allows to access CapitalOnline Cloud Service such as GIC,GBS,GPN and manage your resources easily.

It is based on the official [Open API](https://github.com/capitalonline/openapi/blob/master/README.md).

## Features

You can find all the available actions from [here](https://github.com/capitalonline/openapi/blob/master/%E9%A6%96%E4%BA%91OpenAPI(v1.2).md). List some of them below:

- [X] Instance Management
  - [X] CreateInstance
  - [X] DeleteInstance
  - [X] StopInstance
  - [X] RebootInstance
  - [X] ModifyInstanceChargeType
- [X] Virtual Datacenter Management
  - [X] DescribeVdc
  - [X] CreateVdc
  - [X] DeleteVdc
  - [X] CreatePublicNetwork
  - [X] CreatePrivateNetwork
- [X] Security Group Management
  - [X] CreateSecurityGroup
  - [X] DeleteSecurityGroup
  - [X] ForceDeleteSecurityGroup
  - [X] DescribeSecurityGroupAttribute
  - [X] ModifySecurityGroupAttribute
- [X] And More

## Installation

Use `go get` to install SDK：

```sh
$ go get -u github.com/capitalonline/cds-gic-sdk-go
```

## Examples

```go
    	// Init a credential with Access Key Id and Secret Access Key
	// You can apply them from the CDS web portal
	credential := common.NewCredential(
		os.Getenv("CDS_SECRET_ID"),
		os.Getenv("CDS_SECRET_KEY"),
	)

	// init a client profile with method type
	cpf := profile.NewClientProfile()

	// Example: Get vdc
	cpf.HttpProfile.ReqMethod = "GET"
	vdcClient, _ := vdc.NewClient(credential, "", cpf)
	descVdcRequest := vdc.DescribeVdcRequest()

	// comment out the line below, you can get all the vdc data
	descVdcRequest.RegionId = common.StringPtr(Shanghai)
	descVdcResponse, err := vdcClient.DescribeVdc(descVdcRequest)
	if err != nil {
		fmt.Println("API request fail:", err.Error())
	} else {
		fmt.Println(descVdcResponse.ToJsonString())
	}

	// Example: Create vdc
	cpf.HttpProfile.ReqMethod = "POST"
	vdcClient, _ = vdc.NewClient(credential, "", cpf)
	createVdcRequest := vdc.NewAddVdcRequest()
	createVdcRequest.RegionId = common.StringPtr(Beijing)
	createVdcRequest.VdcName = common.StringPtr("beijing-vdc")
	createVdcRequest.PublicNetwork = &vdc.PublicNetwork{
		// the account of public IP
		IPNum: common.IntPtr(8),
		// the bandwidth of public network
		Qos:           common.IntPtr(10),
		Name:          common.StringPtr("pubnet"),
		BillingMethod: common.StringPtr("Bandwidth"),
		Type:          common.StringPtr("Bandwidth_BGP"),
	}
	createVdcResponse, err := vdcClient.CreateVdc(createVdcRequest)
	if err != nil {
		fmt.Println("API request fail:", err.Error())
	} else {
		fmt.Printf("Task: %v, code: %v", *createVdcResponse.TaskId, *createVdcResponse.Code)
	}
```

Find more from [example](./example).

## Contributing

We work hard to provide a high-quality and useful SDK for CapitalOnline Cloud, and we greatly value feedback and contributions from our community. Please submit your issuesor pull requests through GitHub.

## References

- [CDS OpenAPI Explorer](https://github.com/capitalonline/openapi)

## License

[Apache License v2.0](./LICENSE)
