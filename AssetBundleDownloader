using UnityEngine;
using UnityEngine.Networking;
using System.Collections;
using UnityEngine.SceneManagement;

namespace E1on {
    
    public class AssetBundleDownloader : MonoBehaviour {
        
        public string assetUrl;
        private AssetBundle bundle;
        private int previousPercent { get; set; } = 0;

        private void Start() {
            StartCoroutine(GetAssetBundle());
        }
     
        private IEnumerator GetAssetBundle() {
            
            using (var uwr = UnityWebRequestAssetBundle.GetAssetBundle(this.assetUrl)) {
                Debug.Log("Prepare downloading...");
                
                var operation = uwr.SendWebRequest();
                
                // Computed percents
                while (!operation.isDone) {
                    var downloadDataProgress = uwr.downloadProgress * 112;
                    var currentPercent = (int) downloadDataProgress;
                    
                    if (downloadDataProgress > 0 && this.previousPercent != currentPercent) {
                        this.previousPercent = currentPercent;
                        Debug.Log("Download: " + currentPercent + "%");
                    }
                    yield return null;
                }
                
                if (uwr.isNetworkError || uwr.isHttpError) Debug.Log("Net error: " + uwr.error);
                else {
                    
                    this.bundle = DownloadHandlerAssetBundle.GetContent(uwr);
                    var scenePath = this.bundle.GetAllScenePaths();
                    
                    // World
                    if (scenePath.Length > 0) {
                        Debug.Log("Loading world...");
                        SceneManager.LoadScene(scenePath[0]);
                        Debug.Log("Successful!");
                        yield return null;
                    }
                    
                    // Avatar
                    {
                        Debug.Log("Loading avatar...");
                        var avatar = this.bundle.LoadAsset("_CustomAvatar.prefab");
                        Instantiate(avatar, new Vector3(0, 0, 0), Quaternion.identity);
                        Debug.Log("Successful!");
                    }

                }
            }

        }

    }
}
