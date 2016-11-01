# Retrofit 2.0
## Introduction

>Type-safe HTTP client for Android and Java by Square, Inc.

In `Retrofit 1.x`, there is no direct way to cancel an active task, you need to do it manually, which turns out pretty hard.

In `Retrofit 2.0`, however, `Square` implements this function.

## Import

You can add couple of lines in the `build.gradle` of your module.(You should check the [lastest version](https://github.com/square/retrofit) beforehand.)

```Gradle
compile 'com.squareup.retrofit2:converter-gson:2.1.0'
```

As `Retrofit 2.0` is based on `OkHttp`, we need `OkHttp` as well:

```Gradle
compile 'com.squareup.okhttp3:okhttp:3.4.1'
```

And I will use `Gson` as my Converter:

```Gradle
compile 'com.squareup.retrofit2:converter-gson:2.1.0'
```

## Interface
You need an interface to define the request. Here I use `Githin-Api` to fetch the information of all repos of a specific user.

Annotation is pretty important here, it defines request method, the variable in the url.

```Java
public interface GithubAPIService {

    @GET("/users/{owner}/repos")
    Call<List<RepoInfo>> repoList(
            @Path("owner") String userName
    );

}
```

Note that use `{xxx}` to insert variable into the url and use `Call<T>` to state your request in which you define the variable.

And here `RepoInfo` is a `Java Object` class including the member variable about a repo, just like `repoName`, `description` and so on.

```Java
public class RepoInfo {

    @SerializedName("name")
    public String repoName = null;
    public String description = null;
    @SerializedName("fork")
    public boolean isForked = false;
    @SerializedName("language")
    public String codeLang = null;

    @SerializedName("forks_count")
    public int forkCount = 0;
    @SerializedName("stargazers_count")
    public int StarCount = 0;

}
```

The annotation `SerializedName` is something about `Gson`, you can click [here](https://github.com/Mindjet/Way2Android/blob/master/github-api.md#decode-data) to see how it works.

## Send Request
First of all, you need to instantiate the Retrofit：

```Java
GithubAPIService githubAPIService = mRetrofit.create(GithubAPIService.class);
```

Then, instantiate the call and put it into a queue:

```Java
final Call<List<RepoInfo>> call = githubAPIService.repoList(userName);
call.enqueue(new Callback<List<RepoInfo>>() {
    @Override
    public void onResponse(Call<List<RepoInfo>> call, Response<List<RepoInfo>> response) {

        for (RepoInfo repoInfo : response.body()) {
            Log.d("TAG", repoInfo.repoName);
        }
    }

    @Override
    public void onFailure(Call<List<RepoInfo>> call, Throwable t) {

    }
});
```

Attention, the method `onResponse` will still be invoked if the response can't be decoded, and the `response.body()` will be null. And the method will also be invoked even if there is a `404` error, and in this case we can get the error information from `response.errorBody()`.

## Cancel Request
As I say at the first of this post, a request can be easily cancelled:

```Java
call.cancel();
```

## Source code
You can check the source code in [GithubApiService.java](https://github.com/Mindjet/NetworkThirdPartyLib/blob/master/app/src/main/java/com/mindjet/networkthirdpartylib/GithubAPIService.java) and [RetrofitActivity.java](https://github.com/Mindjet/NetworkThirdPartyLib/blob/master/app/src/main/java/com/mindjet/networkthirdpartylib/RetrofitActivity.java) in my another repo.



