# Template 

The template file is created by the user, and the template file takes the format required by the text / template package of the Go language. You need to write your template using the data in the API specification according to your programming language specification and format.

## Template Content

When you use Snips, you should specify template content and its content structure is fixed as well.

You should manage template content as follow: 

``` sh
└──template
      └──manifest.json
      └──service.tmpl
      └──shared.tmpl
      └──sub_service.tmpl
      └──types.tmpl
```
## Template Files And Template Configuration Files

Snips template files includes `service.tmpl`, `shared.tmpl`, `sub_service.tmpl` and `types.tmpl`. And the configuration file `manifest`.

### Template Configuration Files

The template configuration file supports json format and yaml format, which can be named `manifest.json` or` manifest.yaml`. This file is used to specify the path of the generated file, the naming style of the code, the suffix name, and the file path of each template file and the additional prefix and suffix of the code file generated by the corresponding template file.
 
 Configuration file format:

- ` output` collection is used to configure the common attributes of all output files. `file_naming` collection is used to configure the path of the output file and the naming style for the code. `style` under the` file_naming` collection specifies the naming style of output code file, including  camelcase( "camel_case") and snakecase( "snake_case"). `extension` specifies the suffix name of the output file. `output` collection should be only one.
- The subset in the `template_files` collection specifies the special properties of the individual output files. Each subcollection needs to be named as a template file name. `file_path` in the subset specifies the path to the template file. The` prefix` attribute in `output_file_naming` specifies the prefix of the output file name. The` suffix` attribute specifies the suffix for the output file name.
 
 For example:
 
For json format:
 
``` json
{
  "output": {
    "file_naming": {
      "style": "camel_case",
      "extension": ".go"
    }
  },
  "template_files": {
    "service": {
      "file_path": "service.tmpl",
      "output_file_naming": {
        "prefix": "",
        "suffix": ""
      }
    },
    "sub_service": {
      "file_path": "sub_services.tmpl",
      "output_file_naming": {
        "prefix": "",
        "suffix": ""
      }
    },
    "types": {
      "file_path": "types.tmpl",
      "output_file_naming": {
        "prefix": "",
        "suffix": ""
      }
    }
  }
}
```

For yaml format:

```
template:
  # Template files format
  # Validate formats: Go
  # Default: Go
  format: Go

# Output
output:
  file_naming:
    # Naming style to use in the output file.
    # Available styles: snake_case, camel_case
    # Default: snake_case
    # Example: bucket_acl (snake_case), BucketACL (camel_case)
    style: snake_case
    extension: .go

# Template files to read and execute.
#   Currently, there's are three of them.
template_files:
  # Shared template file.
  # This file will be concatenated with each other template to provide shared
  # nested template definitions.
  shared:
    # Relative file path to load the shared template file.
    # Default: shared.tmpl
    file_path: shared.tmpl

  # Service template file.
  # In this case, a file named "qs_qingstor_service.go" will be generated.
  service:
    # Relative file path to load service template file.
    # Default: service.tmpl
    file_path: service.tmpl
    # Naming options for output file.
    output_file_naming:
      prefix: qs_
      suffix: _service

  # Service template file.
  # In this case, multiple files named like "qs_bucket_sub_service.go"
  # will be generated.
  sub_service:
    # Relative file path to load sub service template file.
    # Default: sub_service.tmpl
    file_path: sub_service.tmpl
    output_file_naming:
      prefix: qs_
      suffix: _sub_service
  # Types template file.
  # In this case, a file named "qs_types.go" will be generated.
  types:
    # Relative file path to load types template file.
    # Default: types.tmpl
    file_path: types.tmpl
    output_file_naming:
      prefix: qs_
      suffix:

# Supporting files to copy directly.
supporting_files:
  - utils.go
  - utils_test.go
  - README.md
```

### Template Files

The template file is used to read API specification files and organize them into code files for the corresponding programming language. The template file should be written using the go template language with the suffix ".tmpl". Snips template files includes `service.tmpl`, `shared.tmpl`, `sub_service.tmpl`, `types.tmpl`.

### service.tmpl

`service.tmpl` is used to generate the code file  towards to service. Snips will generate a code file named by this service according to this template file.In this template, you should specify the template to generate follow codes:

- Initialization function of service.
- Structures or classes towards to service.
- Input and output functions towards to structures or classes.

**Example:** In the case of the QingStor Go SDK ([qingstor-sdk-go] (https://github.com/yunify/qingstor-sdk-go)), the template file is used to generate [qingstor.go] (https: // Github.com/yunify/qingstor-sdk-go/tree/master/service/qingstor.go) code file. In the SDK template file [service.tmpl] (https://github.com/yunify/qingstor-sdk-go/blob/master/template/service.tmpl), contains the structure of the qingstor service, the initialization function and the input and output functions of the QingStor service operation `ListBucket`.

#### shared.tmpl

`Shared.tmpl` contains functions that defines the specification and format of the data organization, which is a collection of functions to be called by the other three template files. The template function requires a large number of references to the structure in Snips that holds the API specification data.

#### sub_service.tmpl

`sub_service.tmpl`is used to generate the code file  towards to sub service.Snips will generate a code file named by this sub service name according to this template.In this template,you should specify the template to generate follow codes:

- Initalization functions of sub service.
- Structures or classes towards to sub service.
- Input and output functions towards to structures or classes.

**Example:** In the QingStor Go SDK ([qingstor-sdk-go] (https://github.com/yunify/qingstor-sdk-go)), this template file is used to generate [bucket.go] (https: // Github.com/yunify/qingstor-sdk-go/tree/master/service/bucket.go) code file. In the SDK's template file [sub_service.tmpl] (https://github.com/yunify/qingstor-sdk-go/blob/master/template/sub_service.tmpl), including the initialization function of the QingStor bucket and the bucket structure of the service operation and its input and output functions.

#### types.tmpl

`Types.tmpl` is the template file used to generate the service custom type code file. These custom types of variables are used to store the various data sets of the service. Snips will generate code files named types based on the template file.

**Example:** In the QingStor Go SDK ([qingstor-sdk-go] (https://github.com/yunify/qingstor-sdk-go)), the template file is used to generate [types.go] (https: // Github.com/yunify/qingstor-sdk-go/tree/master/service/types.go) code file. In the template file [types.tmpl] (https://github.com/yunify/qingstor-sdk-go/blob/master/template/types.tmpl) of the SDK, including the QingStor service's  and the bucket subservice's Customized type.