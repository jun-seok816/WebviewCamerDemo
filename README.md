# GalleryActivity

# setUpload()메소드

## Description 

 - 업로드할 사진을 설정합니다.

## Parameter

- 없음
    
## Return 

 - type : void
 
 - value : 없음

## Dependence function

- intent.putExtra()
  - 인텐트에 확장 데이터를 추가합니다.
  - 지정된 문자열에 데이터를 넣습니다.
  - https://developer.android.com/reference/android/content/Intent#putExtra(java.lang.String,%20boolean)
  
- setResult()
  - Activity에서 돌려받을 결과값을 설정합니다.
  - https://developer.android.com/reference/android/app/Activity#setResult(int,%20android.content.Intent)
  

## Source code 

```
private void setUpload() {
        findViewById(R.id.btn_upload).setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View view) {
                if(selectedIndex == -1){
                    Toast.makeText(GalleryActivity.this, getString(R.string.please_select_image_to_upload), Toast.LENGTH_SHORT).show();
                    return;
                }
                Intent intent = new Intent();
                intent.putExtra(Constants.PATH_OF_IMAGE_TO_UPLOAD, selectedImage);
                setResult(202, intent);
                selectedImage = null;
                selectedIndex = -1;
                finish();
            }
        });
    }
```

# setDelete()메소드

## Description 

 - 삭제할 사진을 설정합니다.

## Parameter

- 없음
    
## Return 

 - type : void
 
 - value : 없음
 
## Constructor

- AlertDialog.Builder
  - 기본 경고 대화 상자 테마를 사용하는 경고 대화 상자의 빌더를 만듭니다.
  - https://developer.android.com/reference/android/app/AlertDialog.Builder

## Dependence function

- alertClickImages.setPositiveButton()
  - 대화 상자의 Ok 버튼을 누를 때 호출 할 리스너를 설정합니다.
  
- imageView.setImageDrawable(null) 
  - Drawable 화면에 그릴 수 있는 그래픽의 일반적인 개념
  - Drawable의 내용을 지우기위해 null값을 넣음
  - https://developer.android.com/reference/android/widget/ImageView#setImageDrawable(android.graphics.drawable.Drawable)
- alertClickImages.setNegativeButton()
  - 대화 상자의 No 버튼을 누를 때 호출 할 리스너를 설정합니다.

## Source code

```
 private void setDelete() {
        findViewById(R.id.btn_delete).setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View view) {
                if(selectedIndex == -1){
                    Toast.makeText(GalleryActivity.this, getString(R.string.please_select_image_to_delete), Toast.LENGTH_SHORT).show();
                    return;
                }
                // Display alert dialog to confirm delete
                AlertDialog.Builder alertClickImages = new AlertDialog.Builder(GalleryActivity.this);
                alertClickImages.setMessage(getString(R.string.are_you_sure_to_delete));
                alertClickImages.setCancelable(true);

                alertClickImages.setPositiveButton(
                        "Yes",
                        new DialogInterface.OnClickListener() {
                            public void onClick(DialogInterface dialog, int id) {
                                File file = new File(selectedImage);
                                boolean isDelete = file.delete();

                                recyclerViewAdapter.remove(selectedIndex);
                                ImageView imageView = (ImageView) findViewById(R.id.image_preview);
                                imageView.setImageDrawable(null);
                                dialog.cancel();
                                selectedImage = null;
                                selectedIndex = -1;
                            }
                        });

                alertClickImages.setNegativeButton(
                        "No",
                        new DialogInterface.OnClickListener() {
                            public void onClick(DialogInterface dialog, int id) {
                                dialog.cancel();
                            }
                        });
                alertClickImages.show();
            }
        });
    }
```

# setDelete()메소드

## Description 

 - 삭제할 사진을 설정합니다.

## Parameter

- 없음
    
## Return 

 - type : void
 
 - value : 없음
 
## Constructor

- new File()
  - 지정된 인스턴스를 추상화된 파일의 경로로 설정한다
  - https://developer.android.com/reference/java/io/File

## Dependence function

- targetDirector.listFiles()
  - targetDirector의 디렉토리의 파일을 표시하는 추상 경로명의 배열을 리턴합니다.
  - https://developer.android.com/reference/java/io/File#listFiles()
  
- Uri.fromeFile()
  - 파일에서 Uri를 만듭니다.
  - https://developer.android.com/reference/android/net/Uri#fromFile(java.io.File)

- recyclerViewAdapter.getItemCount()
  - Adapter의 총 항목 수를 리턴합니다.
  - https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.Adapter#getItemCount()
  - Adapter란
  - 두 개의 상이한 부분을 연결시키는 데 사용하는 장치를 의미한다.
  - 데이터 항목들의 엑세스를 제공합니다.
  - http://blog.naver.com/PostView.nhn?blogId=kookh1&logNo=120170877682
  - https://developer.android.com/reference/android/widget/Adapter


# showNoImagesText()메소드

## Description 

 - 이미지를 찾지 못했을때 사용자에게 Text문자로 찾지 못했다고 알려줍니다.
 
## Parameter

- 없음
    
## Return 

 - type : void
 
 - value : 없음

## Dependence function

- findViewById(R.id.txt_no_images_found).setVisibility(View.VISIBLE);
  - findViewById(R.id.txt_no_images_found)의 가시성 상태를 VISIBLE로 합니다.
- findViewById(R.id.image_preview).setVisibility(View.GONE);
  - findViewById(R.id.image_preview)의 가시성 상태를 GONE로 합니다.
  
# onItemClick()메소드

## Description 

- AdapterView의 항목을 클릭했을 때 호출되는 콜백 메서드입니다.
  - https://developer.android.com/reference/android/widget/AdapterView

## Parameter

- AdapterView
  - 클릭이 발생한 AdapterView입니다.
  
- position  
  - 어댑터에서 view의 위치입니다.
  - https://developer.android.com/reference/android/widget/AdapterView.OnItemClickListener#public-methods_1

## Return 

 - type : void
 
 - value : 없음

## Dependence function

- item.getItemUri()
  - 아래에 설명 참조
- Util.convertImageFileToBitmap() 
  - Util클래스설명 참조
- imageView.setImageBitmap()  
  - imageView의 내용으로 Bitmap을 설정합니다.
- Bitmap  
  - 이미지를 구성하는 단위

# GalleryRecyclerViewAdapter 클래스

# GalleryRecyclerViewAdapter 메소드

## Description 

- GalleryRecyclerViewAdapter 객체의 view를 생성해줍니다.
 
## Parameter

- Context context
  - 객체의 현재 상태를 나타내주는 역할을 함
  - https://velog.io/@allocproc/android-Context%EB%9E%80
    
## Return 

 - type : void
 
 - value : 없음

## Dependence function

- LayoutInflater
  - XML 파일을 해당 View 객체 로 인스턴스화 합니다. 

- LayoutInflater.from(contxt)
  - 지정된 context에서 LayoutInflater를 가져옵니다.
  - https://developer.android.com/reference/android/view/LayoutInflater#from(android.content.Context)
  
## Constructor

- new ArrayList<Uri>
  - ArrayList는 List 인터페이스를 상속받은 클래스로 크기가 가변적으로 변하는 리스트입니다.
  - Uri타입 데이터면 추가 수정 삭제 할 수 있습니다.
  - https://developer.android.com/reference/kotlin/java/util/ArrayList
 
## Sorce code

```
public GalleryRecyclerViewAdapter(Context context) {
        this.context = context;
        layoutInflater = LayoutInflater.from(context);
        itemsUri = new ArrayList<Uri>();
        galleryActivity = (GalleryActivity)context;
    }
```

# onCreateViewHolder 메소드

## Description 

- Called when RecyclerView needs a new RecyclerView.ViewHolder of the given type to represent an item.
- View들이 정리된 리스트에서 각 View를 보관하는 객체를 리턴합니다 
- https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.Adapter#onCreateViewHolder(android.view.ViewGroup,%20int)

## Parameter

- ViewGrop parent
  - View들의 그룹
- int viewType

## Return 

- type : GalleryRecyclerViewAdapter.ItemHolder

- value :ItemHolder

## Dependence function

- layoutInflater.inflate
  - 지정된 Xml 요소들을 객체화시킵니다.
  - https://developer.android.com/reference/android/view/LayoutInflater#inflate(int,%20android.view.ViewGroup)
  
## Source code

```
@Override
    public GalleryRecyclerViewAdapter.ItemHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        CardView itemCardView = (CardView)layoutInflater.inflate(R.layout.layout_cardview, parent, false);
        return new ItemHolder(itemCardView, this);
    }
```

# onBindViewHolder메소드

## Description 

- 리스트에서 데이터를 참조하여 받아온 이미지의 위치설정

## Parameter

- GalleryRecyclerViewAdapter.ItemHolder holder
  - 리스트들의 데이터들을 보유하고있는 변수
- int position
  - 이미지의 위치
  
## Retrun
- type : void
 
- value : 없음
 
## Dependence function

- holder.setItemUri
  - 아래의 설명참조

- holder.setImageView
  - 받은 데이터를 참조하여 이미지를 지정한 위치에 위치시킨다.
  
## Source code

```
@Override
    public void onBindViewHolder(GalleryRecyclerViewAdapter.ItemHolder holder, int position) {
        Uri targetUri = itemsUri.get(position);
        holder.setItemUri(targetUri.getPath());

        if (targetUri != null) {

            try {
                holder.setImageView(Util.convertImageFileToBitmap(targetUri.getPath(), 75, 85));
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
```

# getItemCount 메소드

## Description 

- 어댑터가 보유한 데이터의 총 항목 수를 리턴합니다
  - https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.Adapter#getItemCount()
  
## Parameter

- 없음

## Retrun
- type : void
 
- value : 없음
 
## Dependence function

- itemUri.size()
  - 리스트의 컬렉션프레임워크 타입의 길이를 알려줍니다.
  - https://developer.android.com/reference/android/util/Size
  - https://mine-it-record.tistory.com/126

- 컬렉션프레임워크란
  - 다수의 데이터를 쉽고 효과적으로 처리할 수 있는 표준화된 방법
  
## source code

```
 @Override
    public int getItemCount() {
        return itemsUri.size();
    }
```
  
# setOnItemClickListener

## Description 

- AdapterView의 항목을 클릭했을 때 호출되는 콜백 메소드
  - https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.Adapter#getItemCount()
  
## Parameter

- OnItemClickListener listener
  - The callback that will be invoked. This value may be null.
  - https://developer.android.com/reference/android/widget/AdapterView#setOnItemClickListener(android.widget.AdapterView.OnItemClickListener)
  
## Retrun
- type : void
 
- value : 없음
 
## Source code

```
public void setOnItemClickListener(OnItemClickListener listener) {
        onItemClickListener = listener;
    }
```

# getOnItemClickListener()메소드

## Description 

- AdapterView의 항목과 함께 호출 될 콜백이 클릭되었거나 콜백이 설정되지 않은 경우 null입니다.
  - 콜백이 호출할때 발생하는 값을 리턴합니다.
  - https://developer.android.com/reference/android/widget/AdapterView#getOnItemClickListener()
  
## Parameter

- 없음

## Retrun

- type : void
 
- value : 없음
 
## Source code

```
    public OnItemClickListener getOnItemClickListener() {
        return onItemClickListener;
    }
```

# remove()메소드

## Description 

- 리스트의 데이터를 삭제합니다.

## Parameter

- int selectedIndex
  - 제거할 요소의 인덱스

## Retrun

- type : void
 
- value : 없음

## Dependence function

- itemsUri.remove()
  - 지정된 위치의 요소를 제거합니다.
  - https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.Adapter#notifyItemRemoved(int)

- notifyItemRemoved()
  - 사용자에게 이전에 위치한 항목position이 리스트에서 제거되었음을 알립니다.
  - https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.Adapter#notifyItemRemoved(int)
  
-  galleryActivity.showNoImagesText()
   - 리스트가 없으면 사용자에게 보여줄 이미지가 없다고 알립니다
 
## Source code

```
 public void remove(int selectedIndex) {
        itemsUri.remove(selectedIndex);
        notifyItemRemoved(selectedIndex);

        if(itemsUri.size() == 0){
            galleryActivity.showNoImagesText();
        }
    }
```

# OnItemClickListener 인터페이스

## Description 

- onItemClick메소드를 위한 OnItemClickListener인터페이스이다.

## Source code

```
  public interface OnItemClickListener {
        public void onItemClick(ItemHolder item, int position);
    }
```

# add()메소드

## Description 

- 목록의 지정된 위치에 지정된 요소를 삽입합니다.
  - https://developer.android.com/reference/java/util/ArrayList#add(int,%20E)
  
## Parameter

- int location
  - 지정된 요소가 삽입 될 인덱스
  - https://developer.android.com/reference/java/util/ArrayList#add(int,%20E)

- Uri uri
  - 삽입 할 요소
  - https://developer.android.com/reference/java/util/ArrayList#add(int,%20E)

## Retrun

- type : void
 
- value : 없음

## Dependence function

- notifyItemInserted()
  - 사용자에게 항목 position 이 새로 삽입되었음을 알립니다
  - https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView.Adapter#notifyItemInserted(int)
  - position: 데이터 리스트에서 새로 삽입된 항목의 위치
  

## Source code

```
public void add(int location, Uri iUri) {
        itemsUri.add(location, iUri);
        notifyItemInserted(location);
    }
```

# ItemHolder클래스 

# ItemHolder() 메소드

## Description 

- 변수 imageView, cardView, parent를 초기화시킨다

## Parameter
  
- CardView cardView
  - 둥근 모서리 배경과 그림자가있는 FrameLayout입니다.
  - https://developer.android.com/reference/androidx/cardview/widget/CardView?hl=en

- GalleryRecyclerViewAdapter parent

## Retrun

- type : void
 
- value : 없음

## Dependence function

- itemView.setOnClickListener(this);
  - 뷰항목이 클릭되었을때 onClick 메소드를 콜백한다.
  - https://developer.android.com/reference/android/view/View#setOnClickListener(android.view.View.OnClickListener)
  
## Source code

```
  public ItemHolder(CardView cardView, GalleryRecyclerViewAdapter parent) {
            super(cardView);
            itemView.setOnClickListener(this);
            this.cardView = cardView;
            this.parent = parent;
            imageView = (ImageView) cardView.findViewById(R.id.item_image);
        }
```

# setItemUri()메소드

## Description 

- 리스트 itemuri 배열을 String 문자열 형태로 초기화 시킵니다.

## Parameter
  
- String itemUri

## Retrun

- type : void
 
- value : 없음

## Source code

```
 public void setItemUri(String itemUri){
            this.itemUri = itemUri;
        }
```

# getItemUri()메소드

## Description 

- 문자열로 변환한 itemUri배열 리턴

## Parameter
  
- 없음

## Retrun

- type : String
 
- value : String itemUri

## Source code

```
public String getItemUri(){
            return itemUri;
        }
```

# setImageView()메소드

## Description

- 이미지뷰의 내용으로 비트맵을 설정함

## Parameter
  
- 없음

## Retrun

- type : void
 
- value : 

## Dependence function

- imageView.setImageBitmap()
  - ImageView의 내용으로 Bitmap을 설정합니다.
  - https://developer.android.com/reference/android/widget/ImageView#setImageBitmap(android.graphics.Bitmap)
  
## Source code

```
public void setImageView(Bitmap bitmap){
            imageView.setImageBitmap(bitmap);
        }
```

# onClick메소드

## Description

- 뷰항목이 클릭되면 호출됩니다.

## Parameter
  
- View v
  - 뷰클래스의 변수

## Retrun

- type : void
 
- value : 없음

## Dependence function

- parent.getOnItemClickListener()
  - AdapterView의 항목과 함께 호출 될 콜백이 클릭되었거나 콜백이 설정되지 않은 경우 null값을 호출함
  - https://developer.android.com/reference/android/widget/AdapterView#getOnItemClickListener()
  
- listener.onItemClick()
  - AdapterView의 항목을 클릭했을 때 호출되는 콜백 메서드
  - 선택한 항목과 관련된 데이터에 엑세스해야하는경우 getItemAtPosition (position)을 호출 할 수 있습니다.
  - https://developer.android.com/reference/android/widget/AdapterView.OnItemClickListener#public-methods_1
  
## Source code

```
  @Override
        public void onClick(View v) {
            final OnItemClickListener listener = parent.getOnItemClickListener();
            if(listener != null){
                listener.onItemClick(this, getLayoutPosition());
                //or use
                //listener.onItemClick(this, getAdapterPosition());
            }
```


