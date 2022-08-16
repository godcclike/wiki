# Winserver Information

## SG1 Windows
- Public DNS：ec2-35-72-181-243.ap-northeast-1.compute.amazonaws.com
- User name：	Administrator
- Password： D)a%r7=lCGRSxInawJB2xEYo.7R&jyKv

## Routers and Servers
- 200.32.1.1 - SG1 Gateway
    > - Username: admin
    > - Password: admin

- 200.32.1.8 - Adapter
- 200.32.1.16 - Apps
- 200.32.1.24 - DB
    > - Username: root
    > - Password: Qq!!1234@@

- 200.32.1.25 - ECN Server 1 (bottom)
    > - Username: USERID
    > - Password: P@ssw0rd123

- 200.32.1.26 - ECN Server 2 (upper)
    > - Username: USERID
    > - Password: P@ssw0rd123

## Office server
- 192.168.0.10
    > - Username: platinum
    > - Password: Welcome1

## Office router
- 192.168.0.1
    > - Username: Admin
    > - Password: Platinum@1

## AWS web credentials
> - IAM: platinum-dbs
> - Username: mingxuan.zhang
> - Password： #~4FG#n@x7

> - IAM: platinum-dbs
> - Username: sh-devops
> - Password： lx)o9HEMNH4*NR#1

## Root User Account
> - Username: aws_sin_01@platinumanalytics.net
> - Password： Welcome1234

## Root User Account
> - Username: mingxuan.zhang@plantinumanalytics.net
> - Password： Welcome1234

## Godaddy Account
> - Username: qihong.bao@platinumanalytics.net
> - Password： Platinum123!
> - Domain: platinumecn.com

## ECN Web account (DBS)
- http://200.32.1.16:8082/
- https://ecn.platinumecn.com
> - Trader account: huangyi
> - Password: Platinum1

> - Monitoring account: ECN_Monitor
> - Password: Welcome1

## ECN Web account (SCB)
- http://180.169.143.107:10096/monitor
> - Trader account: ecn_tester
> - Password: 123

## Colt support contact numbers
- +44 207 862 6087
- SG +65 800 852 3528 (Toll free) / +81 3 5617 2342 (Direct)
- Email: TechnicalSupport.UK@colt.net

## Access AWS instances
```shell
aws ec2 describe-instances --filter "Name=tag:Name,Values=Gitlab-Runner" --query 'Reservations[].Instances[].[InstanceId]'          #to get the instance id (can also get from ec2 instance)
aws ssm start-session --target i-08301447abe752fef
```

## OCBC AWS repo
> - Username: mingxuan.zhang-at-135793799950
> - Password: wKzUWJaYeeOc5lZtqGPq7tbr4hDB+fMnuFAcchbTQN8=