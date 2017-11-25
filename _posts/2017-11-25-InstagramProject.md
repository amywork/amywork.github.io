---
layout: post
title: "Instagram 만들어보기"
author: "younari"
---

> 인스타그램과 비슷한 컨셉으로, 유저가 이미지 피커를 통해 사진과 콘텐츠를 포스팅하면 해당 포스트를 파이어베이스 DB에 업로드하고, 메인 테이블뷰에서는 유저의 모든 포스트를 디스플레이 합니다. 메인 테이블뷰에서 포스트 cell을 삭제할 수 있고, 댓글 버튼을 클릭하면 해당 포스트의 댓글 페이지가 네비게이션 구조로 push 됩니다. 

- **프로젝트 기간**: 4days
- **프로젝트 파일**: [깃헙 Onstagram Xcode 프로젝트 바로가기](https://github.com/younari/tastySwift/tree/master/1126_Onstagram)


# 핵심 코드

## 01. 데이터 구조 설계
- 한 명의 유저 uid를 기준으로, 하위에 닉네임/프로필이미지URL/상태메시지/포스트/코멘트를 트리구조로 설계했습니다.
- 포스트 하위에는 .childbyAutoId로 생성한 키값을 기준으로 유저의 포스트 콘텐츠(String) 및 포스트이미지URL(String)이 맵핑됩니다.
- 코멘트 하위에는 .childbyAutoId로 생성한 키값을 기준으로, 포스트 키에 해당하는 코멘트 body들이 코멘트당 고유의 키값을 가지고 맵핑됩니다.


```
"rp6LF5GL43Na81x6vhwxLWPci4t2" : { // uid
    "Comment" : { // comment 노드
      "-Kzffw3NoZewPq4XiBZ_" : { // postID - 1
        "-KzffxVcYWjbA1bd1TmQ" : { // commentID - 1
          "body" : "포스트 ID Kzffw3NoZewPq4XiBZ_의 댓글 1"
        }
      },
      "-KzhS1RLmzKpYOu2O2qu" : { // postID - 2
        "-KzhSLatlzN8mX0uT5yZ" : { // commentID - 2
          "body" : "포스트 ID KzhS1RLmzKpYOu2O2qu의 댓글 1"
        },
        "-KzhSLtn3I0I2KmJCGe_" : {
          "body" : "포스트 ID KzhS1RLmzKpYOu2O2qu의 댓글 2"
        }
      }
    },
    "POST" : { // post 노드
      "-KzhT8EdeMjhgEbI6dYc" : { // postID
        "contents" : "포스트에 코멘트를 클릭\n포스트에 밀어서 지우기",
        "post_img_url" : "https://firebasestorage.googleapis.com/v0/b/onstagram-9c051.appspot.com/o/post_imgs%2F8416CE48-089F-4B7B-BDC2-455230E56B73?alt=media&token=9673512a-76d6-4373-a3f4-47b100c67c1d"
      }
    },
    "nickname" : "WOOZIHO",  // nickname 노드
    "profile_img_url" : "https://firebasestorage.googleapis.com/v0/b/onstagram-9c051.appspot.com/o/profile_imgs%2F11520529-3B2D-4B34-BBA6-1BB95C1A8FA1?alt=media&token=e96a1836-750a-47da-bd8a-fbd7475bd4c0", // profile_img_url 노드
    "status" : "Welcome to my world" // status 노드
  }
}

```

## 02. Model

### UserModel

{% highlight swift %}
internal struct UserModel {

    internal var uid: String

    internal var profileImgUrl: String?

    internal var profileImg: UIImage?

    internal var profileImgData: Data? { get }

    internal var nickName: String?

    internal var statusMessage: String?

    internal var posts: [Onstagram.PostModel]

    internal init(uid: String)

    mutating internal func addUserInfo(with snapshot: [String : Any])
}
{% endhighlight %}

### PostModel


{% highlight swift %}
internal struct PostModel {

    internal var contents: String

    internal var postKey: String?

    internal var imgUrl: String?

    internal var image: UIImage?

    internal var imageData: Data? { get }

    internal var comments: [Onstagram.CommentModel]

    internal init(img: UIImage, contents: String)

    internal init(contents: String)

    internal init?(with dic: [String : String])

    mutating internal func addKey(key: String)
}
{% endhighlight %}

### CommentModel

{% highlight swift %}
import Foundation

internal struct CommentModel {

    internal var body: String

    internal var key: String?

    internal init(contents: String)

    internal init?(dic: [String : String])
}
{% endhighlight %}

## 03. Firebase Manager (singleton)

{% highlight swift %}
import Foundation
import Firebase
import FirebaseAuth
import FirebaseStorage
import FirebaseDatabase

internal class FirebaseManager {

    internal static var shared: FirebaseManager

    internal var currentUser: UserModel!

    internal var uid: String { get }

    internal typealias completion = (_ snapshot: Any?) -> Void

    internal func loadCurrentUser(completion: @escaping completion)

    internal func uploadImg(selectedImgData: Data)

    internal typealias uploadKeySuccess = (_ isSucess: Bool, _ postKey: String?) -> Void

    internal func uploadPost(imgData: Data, contents: String, completion: @escaping uploadKeySuccess)

    internal func uploadUserInfo(nickName: String, status: String)

    internal typealias removeCompletion = (_ isSuccess: Bool) -> Void

    internal func removeSinglePost(key: String, completion: @escaping removeCompletion)

    internal func uploadComment(postKey: String, body: String, completion: @escaping uploadKeySuccess)

    internal typealias downloadComments = (_ isSuccess: Bool, _ comments: Any?) -> Void

    internal func loadComments(postKey: String, completion: @escaping downloadComments)
}
{% endhighlight %}

## 04. Main 컨트롤러

{% highlight swift %}
import UIKit
import FirebaseAuth

internal class ExploreVC : OnstagramVC, ImagePickerDelegate {

    internal var postData: [Onstagram.PostModel]

    override internal func viewDidLoad()

    internal func photoselectorDidSelectedImage(_ selectedImage: UIImage)

    internal var tableView: UITableView

    internal var profileImgView: UIImageView

    internal var imgEditBtn: UIButton

    @objc internal func imgEditAction(_ sender: UIButton)

    internal var nickNameLB: UILabel

    internal var statusLB: UILabel

    internal var lineView: UIView

    internal var editProfileBtn: UIButton

    @objc internal func editBtnHandler(_ sender: UIButton)
}

extension ExploreVC : UITableViewDelegate, UITableViewDataSource {

    internal func tableView(_ tableView: UITableView, titleForHeaderInSection section: Int) -> String?

    internal func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int

    internal func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell

    internal func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat

    internal func tableView(_ tableView: UITableView, canEditRowAt indexPath: IndexPath) -> Bool

    internal func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCellEditingStyle, forRowAt indexPath: IndexPath)
}

extension ExploreVC : CommentCellDelegate {

    internal func postCellDidSelectedCommentBtn(_ data: PostModel)
}

{% endhighlight %}

## 05. View (Post Cell)

{% highlight swift %}
import UIKit

protocol CommentCellDelegate {
    func postCellDidSelectedCommentBtn(_ data: PostModel)
}

class PostCell: UITableViewCell {
    
    // cellFromNib
    @IBOutlet weak var postImageView: UIImageView!
    @IBOutlet weak var contentsTextView: UITextView!

    var delegate: CommentCellDelegate?
    
    @IBAction func didTapCommentBtn(_ sender: UIButton) {
        guard let postData = self.postData else { return }
        delegate?.postCellDidSelectedCommentBtn(postData)
    }

    var postData: PostModel? {
        didSet {
            self.contentsTextView.text = postData?.contents
            
            // Image from URL
            if let url = postData?.imgUrl {
                self.postImageView.loadImage(URLstring: url, completion: { (isSuccess) in
                    if !isSuccess {
                        self.postImageView.image = #imageLiteral(resourceName: "NoImage")
                    }
                })
            }
            
            // Image from library
            if let image = postData?.image {
                self.postImageView.image = image
            }
            
        }
    }
    
    // Estimated Cell
    static var cellFromNib: PostCell {
        guard let cell = Bundle.main.loadNibNamed("PostCell", owner: nil, options: nil)?.first as? PostCell else {
            return PostCell()
        }
        return cell
    }
    
}
{% endhighlight %}
