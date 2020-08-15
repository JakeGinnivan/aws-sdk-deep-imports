# aws-sdk-deep-imports

The AWS JS SDK pulls in a lot of type definitions, this is because deep imports transitively pull in all other service clients.

## Performance

This is not extensive testing, but on my 16" MacBook Pro I get this

### Before

```
➜  aws-sdk-deep-imports git:(master) ✗ yarn tsc --extendedDiagnostics
yarn run v1.22.4
$ /Users/jakeginnivan/_code/aws-sdk-deep-imports/node_modules/.bin/tsc --extendedDiagnostics
Files:                         259
Lines:                      439618
Nodes:                     1314793
Identifiers:                483501
Symbols:                    280406
Types:                          85
Instantiations:                  0
Memory used:               375717K
Assignability cache size:        0
Identity cache size:             0
Subtype cache size:              0
Strict subtype cache size:       0
I/O Read time:               0.18s
Parse time:                  2.19s
ResolveTypeReference time:   0.00s
ResolveModule time:          0.18s
Program time:                2.62s
Bind time:                   0.83s
Check time:                  0.01s
transformTime time:          0.01s
commentTime time:            0.00s
I/O Write time:              0.00s
printTime time:              0.02s
Emit time:                   0.02s
Total time:                  3.48s
✨  Done in 4.25s.
```

### After

To try it out, just run `yarn patch-package` to apply the patch to the AWS-SDK itself.

```
➜  aws-sdk-deep-imports git:(master) ✗ yarn tsc --extendedDiagnostics
yarn run v1.22.4
$ /Users/jakeginnivan/_code/aws-sdk-deep-imports/node_modules/.bin/tsc --extendedDiagnostics
Files:                         22
Lines:                      31187
Nodes:                     129088
Identifiers:                47272
Symbols:                    28625
Types:                         85
Instantiations:                 0
Memory used:               54651K
Assignability cache size:       0
Identity cache size:            0
Subtype cache size:             0
Strict subtype cache size:      0
I/O Read time:              0.01s
Parse time:                 0.36s
ResolveTypeReference time:  0.00s
ResolveModule time:         0.05s
Program time:               0.43s
Bind time:                  0.18s
Check time:                 0.01s
transformTime time:         0.01s
commentTime time:           0.00s
I/O Write time:             0.00s
printTime time:             0.01s
Emit time:                  0.01s
Total time:                 0.63s
✨  Done in 0.97s.
```

## Testing

Because skipLibCheck is enabled, this makes sure references have not been broken with the patch
`yarn tsc -p tsconfig.all.json`