# Commands

### ACL の一覧を取得

```bash
# デフォルトのACLのみ取得
$ aws --region ap-northeast-1 ec2 describe-network-acls | jq '.NetworkAcls[] | select(.IsDefault == true) | .NetworkAclId'
```

```bash
# 全リージョンのデフォルトACLを取得
for value in $(aws --region ap-northeast-1 ec2 describe-regions | jq ".Regions[].RegionName" | sed 's/\"//g'); do
  echo ${value},
  aws --region ${value} ec2 describe-network-acls | jq '.NetworkAcls[] | select(.IsDefault == true) | .NetworkAclId'
done
```

```bash
# 全リージョンのACLをtsvで表示 (特定のタグを取得)
for value in $(aws --region ap-northeast-1 ec2 describe-regions | jq ".Regions[].RegionName" | sed 's/\"//g'); do
  aws --region ${value} ec2 describe-network-acls | jq -r '.NetworkAcls[] | [.NetworkAclId, .IsDefault, (.Tags[]?|select(.Key=="keyname")|.Value)] | @tsv' | sed "s/^/${value}\t/"
done
```

```bash
for value in $(aws --region ap-northeast-1 ec2 describe-regions | jq ".Regions[].RegionName" | sed 's/\"//g'); do
  echo ${value}
  acls=$(aws --region ${value} ec2 describe-network-acls | jq -r '.NetworkAcls[] | [.NetworkAclId, .VpcId, .IsDefault, (.Tags[]?|select(.Key=="ManagedBy")|.Value)] | @tsv' | sed "s/\ttrue//" | sed "s/\tfalse/\tNotDefault/")
  vpcs=$(aws --region ${value} ec2 describe-vpcs | jq '.Vpcs[] | {"VpcId": .VpcId, "Tags": [.Tags[]?]}')
  for vpc in $(echo ${vpcs} | jq -r '.VpcId')
  do
    echo ${acls} | grep ${vpc} | sed "s/${vpc}/${vpc}($(echo ${vpcs} | jq -r --arg vpc ${vpc} '. | select(.VpcId== $vpc ) | .Tags[]?|select(.Key=="Name")|.Value'))/"
  done
  echo;
done
```

```bash
# ACLの一覧を取得し、terraform コードに整形
{
echo "locals {"
for value in $(aws --region ap-northeast-1 ec2 describe-regions | jq ".Regions[].RegionName" | sed 's/\"//g'); do
  echo "  acl_ids_$(echo ${value} | sed -r "s/-([0-9])/\1/" | sed "s/-/_/g" | sed "s/north/n/" | sed "s/south/s/" | sed "s/central/c/" | sed "s/east/e/" | sed "s/west/w/") = [ # ${value}"
  acls=$(aws --region ${value} ec2 describe-network-acls | jq -r '.NetworkAcls[] | [.NetworkAclId, .VpcId, .IsDefault, (.Tags[]?|select(.Key=="ManagedBy")|.Value)] | @tsv'| sort -t$'\t' -k2 | sed "s/vpc-/# vpc-/" | sed -r "s/^(acl-[a-z0-9]*)/    \"\1\",/" | sed "s/\ttrue//" | sed "s/\tfalse/\tNotDefault/" | sed -e "$ s/\",\t/\"\t/")
  vpcs=$(aws --region ${value} ec2 describe-vpcs | jq '.Vpcs[] | {"VpcId": .VpcId, "Tags": [.Tags[]?]}')
  for vpc in $(echo ${vpcs} | jq -r '.VpcId' | sort)
  do
    echo ${acls} | grep ${vpc} | sed "s/${vpc}/${vpc}($(echo ${vpcs} | jq -r --arg vpc ${vpc} '. | select(.VpcId== $vpc ) | .Tags[]?|select(.Key=="Name")|.Value'))/"
  done
  echo "  ]"
done
echo "}"
}
```

```bash
{
for value in $(aws --region ap-northeast-1 ec2 describe-regions | jq ".Regions[].RegionName" | sed 's/\"//g'); do
  echo "provider \"aws\" {"
  echo "  alias  = \"$(echo ${value} | sed -r "s/-([0-9])/\1/" | sed "s/-/_/g" | sed "s/north/n/" | sed "s/south/s/" | sed "s/central/c/" | sed "s/east/e/" | sed "s/west/w/")\""
  echo "  region = \"${value}\""
  echo "}"
  echo;
done
}
```


### 参考

[gist - aws cli + jq example](https://gist.github.com/hummus/8592113)
