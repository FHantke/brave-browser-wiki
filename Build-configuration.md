A number of build parameters are passed via .npmrc. The following are used in official release builds:

* All platforms
  * binance_client_id
  * bitflyer_client_id
  * bitflyer_client_secret
  * bitflyer_staging_client_id
  * bitflyer_staging_client_secret
  * bitflyer_staging_url
  * brave_google_api_endpoint
  * brave_google_api_key
  * brave_infura_project_id
  * brave_pref_hash_seed
  * brave_referrals_api_key
  * brave_services_key
  * brave_stats_api_key
  * brave_stats_updater_url
  * brave_sync_endpoint
  * brave_variations_server_url
  * dcheck_always_on (set to 1 in PRs on non-Windows platforms)
  * gemini_api_staging_url
  * gemini_api_url
  * gemini_client_id
  * gemini_client_secret
  * gemini_oauth_staging_url
  * gemini_oauth_url
  * gemini_wallet_client_id
  * gemini_wallet_client_secret
  * gemini_wallet_staging_client_id
  * gemini_wallet_staging_client_secret
  * goma_server_host
  * google_default_client_id
  * google_default_client_secret
  * is_brave_release_build (set to 0 in PRs)
  * rewards_grant_dev_endpoint
  * rewards_grant_prod_endpoint
  * rewards_grant_staging_endpoint
  * safebrowsing_api_endpoint
  * sardine_client_id
  * sardine_client_secret
  * updater_dev_endpoint
  * updater_prod_endpoint
  * uphold_client_id
  * uphold_client_secret
  * uphold_staging_client_id
  * uphold_staging_client_secret
  * brave_p3a_enabled true
  * p3a_json_upload_url
  * p3a_creative_upload_url
  * p2a_json_upload_url
  * p3a_star_upload_url 
  * star_randomness_host
* Android
  * brave_android_developer_options_code
  * brave_android_key_password
  * brave_android_keystore_name
  * brave_android_keystore_password
  * brave_android_keystore_path
  * brave_safebrowsing_api_key
  * brave_safetynet_api_key (not set in PRs)
* macOS
  * notary_password
  * notary_user
  * sparkle_eddsa_private_key
  * sparkle_eddsa_public_key

Internal developers can find the values for these parameters in the company secrets management service (more information at https://github.com/brave/devops/wiki/npm-config-for-Brave-Developers).

Since each of those parameters is tied to a service that Brave has to pay to maintain, or get access to, external developers will have to supply their own values.