
# Welcome to DracoArts

![Logo](https://dracoarts-logo.s3.eu-north-1.amazonaws.com/DracoArts.png)




# Ready Player Me Unity Integration
  Ready Player Me is a cross-platform avatar system that allows users to create and customize 3D avatars for games and applications. The Unity integration enables developers to easily import these avatars into their projects, supporting various customization options, animations, and outfit changes.
# Key Features
✅ Avatar Creation & Customization

- Users can design avatars using the Ready Player Me web tool or in Unity.

- Hundreds of customization options (hairstyles, clothing, facial features, skin tones).

- Supports both full-body and half-body (bust) avatars.

✅ Seamless Unity Integration

- Plug-and-play Unity SDK with prefabs for quick setup.

- Works with Built-in, URP, and HDRP render pipelines.

- Optimized for performance across different platforms.

✅ Cross-Platform Support

- Desktop (Windows, Mac, Linux)

- Mobile (iOS, Android)

- WebGL

- VR/AR (Oculus, Meta Quest, HoloLens, etc.)

✅ Advanced Avatar Features

- Dynamic Outfit Changes – Swap clothing and accessories at runtime.

- Facial Animations – Supports blendshapes for lip-sync and expressions.

- Custom Animations – Import and apply motion-captured animations.

- Avatar Caching – Reduces load times by storing avatars locally.


# How It Works
## 1. Avatar Creation
 - Users create their avatars using:

- The Ready Player Me website (via a web-based configurator).

- An in-Unity avatar creator (if integrated).

## 2. Importing Avatars into Unity
- The Ready Player Me Unity SDK provides an AvatarLoader system to fetch avatars via URL.

- Avatars are automatically rigged and ready for animations.

## 3. Customization & Animation
- Developers can modify outfits, apply animations, and adjust materials at runtime.

- Supports Unity’s Animator Controller for custom animation states.

# Setting Up Ready Player Me in Unity
## 1.  Download & Import the SDK
 ###   Get the Unity SDK from:

[Unity Asset Store](https://assetstore.unity.com/packages/tools/game-toolkits/ready-player-me-avatar-and-character-creator-259814)

 ###  GitHub repo (for advanced use).
 [Package](https://github.com/readyplayerme/rpm-unity-sdk-core/releases)


Import into Unity:

Assets > Import Package > Custom Package

# Documentation & Support
[📖 Official Documentation](https://docs.readyplayer.me/ready-player-me/integration-guides/overview)

💡 [GitHub Repository](https://github.com/readyplayerme/rpm-unity-sdk-core)

🛠 [Support & Community](https://readyplayer.me/)
## Usage/Examples

    [SerializeField][Tooltip("RPM avatar URL or shortcode to load")] 
        private string avatarUrl;
        private GameObject avatar;
        private AvatarObjectLoader avatarObjectLoader;
        [SerializeField][Tooltip("Animator to use on loaded avatar")] 
        private RuntimeAnimatorController animatorController;
        [SerializeField][Tooltip("If true it will try to load avatar from avatarUrl on start")] 
        private bool loadOnStart = true;
        [SerializeField][Tooltip("Preview avatar to display until avatar loads. Will be destroyed after new avatar is loaded")]
        private GameObject previewAvatar;

        public event Action OnLoadComplete;
        
        private void Start()
        {
            avatarObjectLoader = new AvatarObjectLoader();
            avatarObjectLoader.OnCompleted += OnLoadCompleted;
            avatarObjectLoader.OnFailed += OnLoadFailed;
            
            if (previewAvatar != null)
            {
                SetupAvatar(previewAvatar);
            }
            if (loadOnStart)
            {
                LoadAvatar(avatarUrl);
            }
        }

        private void OnLoadFailed(object sender, FailureEventArgs args)
        {
            OnLoadComplete?.Invoke();
        }

        private void OnLoadCompleted(object sender, CompletionEventArgs args)
        {
            if (previewAvatar != null)
            {
                Destroy(previewAvatar);
                previewAvatar = null;
            }
            SetupAvatar(args.Avatar);
            OnLoadComplete?.Invoke();
        }

        private void SetupAvatar(GameObject  targetAvatar)
        {
            if (avatar != null)
            {
                Destroy(avatar);
            }
            
            avatar = targetAvatar;
            // Re-parent and reset transforms
            avatar.transform.parent = transform;
            avatar.transform.localPosition = avatarPositionOffset;
            avatar.transform.localRotation = Quaternion.Euler(0, 0, 0);
            
            var controller = GetComponent<ThirdPersonController>();
            if (controller != null)
            {
                controller.Setup(avatar, animatorController);
            }
        }

        public void LoadAvatar(string url)
        {
            //remove any leading or trailing spaces
            avatarUrl = url.Trim(' ');
            avatarObjectLoader.LoadAvatar(avatarUrl);
        }

    

## Images
## Part 1
![](https://raw.githubusercontent.com/AzharKhemta/DemoClient/refs/heads/main/Ready%20player%20Part%201.gif)

## Part 2
![](https://raw.githubusercontent.com/AzharKhemta/DemoClient/refs/heads/main/Ready%20player%20me%20Part%202.gif)
## Authors

- [@MirHamzaHasan](https://github.com/MirHamzaHasan)
- [@WebSite](https://mirhamzahasan.com)


## 🔗 Links

[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/company/mir-hamza-hasan/posts/?feedView=all/)
## Tech Stack
**Client:** Unity,C#

**Package:** Ready Player Me SDK



