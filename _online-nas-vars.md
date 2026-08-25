{%- if "gov" in build_flags -%}
  {%- set identity_guide_url = "/nhncloud/ko/public-api/iaas-token-gov/" -%}
  {%- set terraform_guide_url = "/nhncloud/ko/terraform-guide-gov/" -%}
  {%- set terraform_support = true -%}
  {%- set encryption = false -%}
  {%- set monitoring = true -%}
  {%- set replication = true -%}
  {%- set regions = [
    {"name": "한국(판교) 리전", "endpoint": "https://kr1-api-nas-infrastructure.gov-nhncloudservice.com"},
    {"name": "한국(평촌) 리전", "endpoint": "https://kr2-api-nas-infrastructure.gov-nhncloudservice.com"},
  ] -%}

{%- elif "ncgn" in build_flags -%}
  {%- set identity_guide_url = "/nhncloud/ko/public-api/iaas-token/" -%}
  {%- set terraform_guide_url = "/Compute/Instance/ko/terraform-guide/" -%}
  {%- set terraform_support = false -%}
  {%- set encryption = false -%}
  {%- set monitoring = true -%}
  {%- set replication = false -%}
  {%- set regions = [
    {"name": "한국(판교) 리전", "endpoint": "https://kr1-api-nas-infrastructure.gncloud.go.kr"},
  ] -%}

{%- elif "ppp" in build_flags -%}
  {%- set terraform_support = false -%}
  {%- set terraform_guide_url = "/Compute/Instance/ko/terraform-guide/" -%}
  {%- set encryption = false -%}
  {%- set monitoring = true -%}
  {%- set replication = false -%}
  {%- if "ninc" in build_flags -%}
    {%- set identity_guide_url = "/Compute/Compute/ko/identity-api-ninc/" -%}
    {%- set regions = [{"name": "한국(대구) 리전", "endpoint": "https://kr4-api-nas-infrastructure.ninc.go.kr"}] -%}
  {%- elif "ngsc" in build_flags -%}
    {%- set identity_guide_url = "/Compute/Compute/ko/identity-api-ngsc/" -%}
    {%- set regions = [{"name": "한국(대구) 리전", "endpoint": "https://kr4-api-nas-infrastructure.ngsc.go.kr"}] -%}
  {%- elif "ngoic" in build_flags -%}
    {%- set identity_guide_url = "/Compute/Compute/ko/identity-api-ngoic/" -%}
    {%- set regions = [{"name": "한국(대구) 리전", "endpoint": "https://kr4-api-nas-infrastructure.ngoic.com"}] -%}
  {%- elif "ngovc" in build_flags -%}
    {%- set identity_guide_url = "/Compute/Compute/ko/identity-api-ngovc/" -%}
    {%- set regions = [{"name": "한국(대구) 리전", "endpoint": "https://kr4-api-nas-infrastructure.ngovc.com"}] -%}
  {%- endif -%}

{%- else -%}
  {#- public (기본값) -#}
  {%- set identity_guide_url = "/nhncloud/ko/public-api/iaas-token/" -%}
  {%- set terraform_guide_url = "/nhncloud/ko/terraform-guide/" -%}
  {%- set terraform_support = true -%}
  {%- set encryption = true -%}
  {%- set monitoring = true -%}
  {%- set replication = true -%}
  {%- set regions = [
    {"name": "한국(판교) 리전", "endpoint": "https://kr1-api-nas-infrastructure.nhncloudservice.com"},
    {"name": "한국(평촌) 리전", "endpoint": "https://kr2-api-nas-infrastructure.nhncloudservice.com"},
    {"name": "한국(광주) 리전", "endpoint": "https://kr3-api-nas-infrastructure.nhncloudservice.com"},
  ] -%}
{%- endif -%}
