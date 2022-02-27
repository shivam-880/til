## List topics
```
pulsar-admin topics list public/default
```

## Delete topic
```
pulsar-admin topics delete persistent://test-tenant/ns1/tp1
```

## Stats
```
pulsar-admin topics stats persistent://test-tenant/ns1/tp1 
```

## Internal Stats
```
pulsar-admin topics stats-internal persistent://test-tenant/ns1/tp1
```

## Peek messages 
```bash
$ pulsar-admin topics peek-messages --count 10 --subscription my-subscription persistent://test-tenant/ns1/tp1 
```

## Get message by id
```bash
$ pulsar-admin topics get-message-by-id persistent://public/default/my-topic -l 10 -e 0
```

## Examin messages
```bash
$ pulsar-admin topics examine-messages persistent://public/default/my-topic -i latest -m 1
```

## Lookup
```bash
$ pulsar-admin topics lookup persistent://test-tenant/ns1/tp1 
```

## Subscriptions
```bash
$ pulsar-admin topics subscriptions persistent://test-tenant/ns1/tp1 
```

## Blocklog size
```bash
$ pulsar-admin topics get-backlog-size -m 1:1 persistent://test-tenant/ns1/tp1-partition-0 
```

## Last message id
```bash
$ pulsar-admin topics last-message-id persistent://public/default/Raphtory_271935070_watermark
```
