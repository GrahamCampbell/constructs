# AWS CDK constructs for PHP on AWS Lambda

The [Bref](https://bref.sh/) CDK constructs let you deploy serverless PHP applications on AWS Lambda using the AWS CDK.

By default, Bref [deploys using the Serverless Framework](https://bref.sh/docs/deploy.html). Using the AWS CDK is an alternative, but be aware that this is an advanced topic. If you are lost, follow the [Bref documentation](https://bref.sh/docs/) instead.

## Installation

Install [the package](https://github.com/brefphp/constructs) with NPM:

```bash
npm install @bref.sh/constructs
```

## Usage

Simple example to deploy an HTTP application:

```typescript
import { Construct } from 'constructs';
import { App, Stack } from 'aws-cdk-lib';
import { PhpFpmFunction } from '@bref.sh/constructs';

class MyStack extends Stack {
    constructor(scope: Construct, id: string, props?: StackProps) {
        super(scope, id, props);

        new PhpFpmFunction(this, 'Hello', {
            handler: 'public/index.php',
        });
    }
}

const app = new App();
new MyStack(app, 'test', {
    env: {
        region: 'eu-west-1',
    },
});
```

## Constructs

### Functions

#### `PhpFpmFunction`

This construct deploys a PHP function with the [HTTP runtime](https://bref.sh/docs/runtimes/http.html).

```typescript
new PhpFpmFunction(this, 'MyFunction', {
    handler: 'public/index.php',
});
```

It inherits from the AWS CDK [`Function` construct](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.aws_lambda.Function.html) with these options set by default:

- `handler`: `index.php` by default
- `runtime`: `provided.al2`
- `code`: the code is automatically zipped from the current directory.
- `layers`: the Bref layer is automatically added.
- `memorySize`: `1024`
- `timeout`: `28` (seconds)

The code is automatically zipped from the current directory. You can override this behavior by setting the `code` property:

```typescript
import { packagePhpCode } from '@bref.sh/constructs';

new PhpFpmFunction(this, 'MyFunction', {
    code: packagePhpCode('custom-path', {
        exclude: ['docs'],
    }),
});
```

The following paths are always excluded: `.git`, `.idea`, `cdk.out`, `node_modules`, `.bref`, `.serverless`, `tests`.

The construct also adds the following options:

- `phpVersion` (default: `8.1`): the PHP version to use.

#### `PhpFunction`

This construct deploys a PHP function with the ["event-driven function" runtime](https://bref.sh/docs/runtimes/function.html).

```typescript
new PhpFunction(this, 'MyFunction', {
    handler: 'my-handler.php',
});
```

It inherits from the AWS CDK [`Function` construct](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.aws_lambda.Function.html) with these options set by default:

- `runtime`: `provided.al2`
- `code`: the code is automatically zipped from the current directory.
- `layers`: the Bref layer is automatically added.
- `memorySize`: `1024`
- `timeout`: `6` (seconds)

The code is automatically zipped from the current directory. You can override this behavior by setting the `code` property:

```typescript
import { packagePhpCode } from '@bref.sh/constructs';

new PhpFunction(this, 'MyFunction', {
    // ...
    code: packagePhpCode('custom-path', {
        exclude: ['docs'],
    }),
});
```

The following paths are always excluded: `.git`, `.idea`, `cdk.out`, `node_modules`, `.bref`, `.serverless`, `tests`.

The construct also adds the following options:

- `phpVersion` (default: `8.1`): the PHP version to use.

#### `ConsoleFunction`

This construct deploys a PHP function with the ["console" runtime](https://bref.sh/docs/runtimes/console.html).

```typescript
new ConsoleFunction(this, 'Artisan', {
    handler: 'artisan',
});
```

It inherits from the AWS CDK [`Function` construct](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.aws_lambda.Function.html) with these options set by default:

- `runtime`: `provided.al2`
- `code`: the code is automatically zipped from the current directory.
- `layers`: the Bref layers are automatically added.
- `memorySize`: `1024`
- `timeout`: `6` (seconds)

The code is automatically zipped from the current directory. You can override this behavior by setting the `code` property:

```typescript
import { packagePhpCode } from '@bref.sh/constructs';

new ConsoleFunction(this, 'Artisan', {
    // ...
    code: packagePhpCode('custom-path', {
        exclude: ['docs'],
    }),
});
```

The following paths are always excluded: `.git`, `.idea`, `cdk.out`, `node_modules`, `.bref`, `.serverless`, `tests`.

The construct also adds the following options:

- `phpVersion` (default: `8.1`): the PHP version to use.
