---
title: フレンド アセンブリ (C#)
ms.date: 03/03/2019
ms.assetid: b65ea7de-0801-477a-a39c-e914c2cc107c
ms.openlocfilehash: 770eb70a5350d7d26e03cb4e605b442100da2a53
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "68671197"
---
# <a name="friend-assemblies"></a><span data-ttu-id="b0847-102">フレンド アセンブリ</span><span class="sxs-lookup"><span data-stu-id="b0847-102">Friend assemblies</span></span>

<span data-ttu-id="b0847-103">"*フレンド アセンブリ*" とは、別のアセンブリに [internal](../../csharp/language-reference/keywords/internal.md) (C# の場合、または Visual Basic では [Friend](../../visual-basic/language-reference/modifiers/friend.md)) として宣言されている型やメンバーにアクセスできるアセンブリです。</span><span class="sxs-lookup"><span data-stu-id="b0847-103">A *friend assembly* is an assembly that can access another assembly's [internal](../../csharp/language-reference/keywords/internal.md) (in C# or [Friend](../../visual-basic/language-reference/modifiers/friend.md) in Visual Basic) types and members.</span></span> <span data-ttu-id="b0847-104">フレンド アセンブリとして指定した場合、public として宣言されていないその型とメンバーに、他のアセンブリからアクセスできるようになります。</span><span class="sxs-lookup"><span data-stu-id="b0847-104">If you identify an assembly as a friend assembly, you no longer have to mark types and members as public in order for them to be accessed by other assemblies.</span></span> <span data-ttu-id="b0847-105">この方法は、特に次の状況で利便性を発揮します。</span><span class="sxs-lookup"><span data-stu-id="b0847-105">This is especially convenient in the following scenarios:</span></span>

- <span data-ttu-id="b0847-106">単体テスト中、テスト コードが別のアセンブリで実行されている状況で、C# では `internal`、または Visual Basic では `Friend` としてマークされる、そのテスト対象のアセンブリ内のメンバーにアクセスしなければならないとき。</span><span class="sxs-lookup"><span data-stu-id="b0847-106">During unit testing, when test code runs in a separate assembly but requires access to members in the assembly being tested that are marked as `internal` in C# or `Friend` in Visual Basic.</span></span>

- <span data-ttu-id="b0847-107">クラス ライブラリの開発中、そのライブラリへの追加機能が別のアセンブリにある状況で、C# では `internal`、または Visual Basic では `Friend` としてマークされる、既存アセンブリ内のメンバーにアクセスしなければならないとき。</span><span class="sxs-lookup"><span data-stu-id="b0847-107">When you are developing a class library and additions to the library are contained in separate assemblies but require access to members in existing assemblies that are marked as `internal` in C# or `Friend` in Visual Basic.</span></span>

## <a name="remarks"></a><span data-ttu-id="b0847-108">解説</span><span class="sxs-lookup"><span data-stu-id="b0847-108">Remarks</span></span>

<span data-ttu-id="b0847-109"><xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性を利用し、特定のアセンブリの 1 つまたは複数のフレンド アセンブリを特定できます。</span><span class="sxs-lookup"><span data-stu-id="b0847-109">You can use the <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> attribute to identify one or more friend assemblies for a given assembly.</span></span> <span data-ttu-id="b0847-110">次の例では、アセンブリ A で <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性を利用し、アセンブリ `AssemblyB` をフレンド アセンブリとして指定しています。</span><span class="sxs-lookup"><span data-stu-id="b0847-110">The following example uses the <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> attribute in assembly A and specifies assembly `AssemblyB` as a friend assembly.</span></span> <span data-ttu-id="b0847-111">これで、アセンブリ `AssemblyB` は、アセンブリ A の中で、C# では `internal`、または Visual Basic では `Friend` としてマークされるすべての型とメンバーにアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="b0847-111">This gives assembly `AssemblyB` access to all types and members in assembly A that are marked as `internal` in C# or `Friend` in Visual Basic.</span></span>

> [!NOTE]
> <span data-ttu-id="b0847-112">別のアセンブリ (アセンブリ *A*) の内部型または内部メンバーにアクセスするアセンブリ (アセンブリ `AssemblyB`) をコンパイルする際は、 **/out** コンパイラ オプションを使用して、出力ファイル (.exe または .dll) の名前を明示的に指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0847-112">When you compile an assembly (assembly `AssemblyB`) that will access internal types or internal members of another assembly (assembly *A*), you must explicitly specify the name of the output file (.exe or .dll) by using the **/out** compiler option.</span></span> <span data-ttu-id="b0847-113">この指定は必ず行ってください。コンパイラが外部参照にバインドする時点ではまだ、ビルド中のアセンブリの名前が生成されていないためです。</span><span class="sxs-lookup"><span data-stu-id="b0847-113">This is required because the compiler has not yet generated the name for the assembly it is building at the time it is binding to external references.</span></span> <span data-ttu-id="b0847-114">詳細については、「[-out (C# コンパイラ オプション)](../../csharp/language-reference/compiler-options/out-compiler-option.md)」または「[-除外 (Visual Basic)](../../visual-basic/reference/command-line-compiler/out.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b0847-114">For more information, see [/out (C#)](../../csharp/language-reference/compiler-options/out-compiler-option.md) or [/out (Visual Basic)](../../visual-basic/reference/command-line-compiler/out.md).</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="b0847-115">C#</span><span class="sxs-lookup"><span data-stu-id="b0847-115">C#</span></span>](#tab/csharp)

```csharp
using System.Runtime.CompilerServices;
using System;

[assembly: InternalsVisibleTo("AssemblyB")]

// The class is internal by default.
class FriendClass
{
    public void Test()
    {
        Console.WriteLine("Sample Class");
    }
}

// Public class that has an internal method.
public class ClassWithFriendMethod
{
    internal void Test()
    {
        Console.WriteLine("Sample Method");
    }

}
```

# <a name="visual-basictabvb"></a>[<span data-ttu-id="b0847-116">Visual Basic</span><span class="sxs-lookup"><span data-stu-id="b0847-116">Visual Basic</span></span>](#tab/vb)

```vb
Imports System.Runtime.CompilerServices
Imports System
<Assembly: InternalsVisibleTo("AssemblyB")>

' Friend class.
Friend Class FriendClass
    Public Sub Test()
        Console.WriteLine("Sample Class")
    End Sub
End Class

' Public class with a Friend method.
Public Class ClassWithFriendMethod
    Friend Sub Test()
        Console.WriteLine("Sample Method")
    End Sub
End Class
```

---

<span data-ttu-id="b0847-117">`internal` 型とメンバー (C# の場合、または Visual Basic では `Friend`) にアクセスできるのは、明示的にフレンドとして指定されたアセンブリだけです。</span><span class="sxs-lookup"><span data-stu-id="b0847-117">Only assemblies that you explicitly specify as friends can access `internal` (in C# or `Friend` in Visual Basic) types and members.</span></span> <span data-ttu-id="b0847-118">たとえば、アセンブリ B がアセンブリ A のフレンドで、アセンブリ C がアセンブリ B を参照している場合、A の `internal` 型 (C# の場合、または Visual Basic では `Friend`) に C からアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="b0847-118">For example, if assembly B is a friend of assembly A and assembly C references assembly B, C does not have access to `internal` (in C# or `Friend` in Visual Basic) types in A.</span></span>

<span data-ttu-id="b0847-119">コンパイラは、<xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性に渡されるフレンド アセンブリ名の基本検証を実行します。</span><span class="sxs-lookup"><span data-stu-id="b0847-119">The compiler performs some basic validation of the friend assembly name passed to the <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> attribute.</span></span> <span data-ttu-id="b0847-120">アセンブリ *A* が *B* をフレンド アセンブリとして宣言する場合、検証規則は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="b0847-120">If assembly *A* declares *B* as a friend assembly, the validation rules are as follows:</span></span>

- <span data-ttu-id="b0847-121">アセンブリ *A* に厳密な名前が付けられている場合、アセンブリ *B* にも厳密な名前が付けられている必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0847-121">If assembly *A* is strong named, assembly *B* must also be strong named.</span></span> <span data-ttu-id="b0847-122">属性に渡されるフレンド アセンブリ名は、アセンブリ名と、アセンブリ *B* の署名に使用される厳密名キーの公開キーで構成されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0847-122">The friend assembly name that is passed to the attribute must consist of the assembly name and the public key of the strong-name key that is used to sign assembly *B*.</span></span>

     <span data-ttu-id="b0847-123"><xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性に渡すフレンド アセンブリ名として、アセンブリ *B* の厳密な名前を指定することはできません。アセンブリのバージョン、カルチャ、アーキテクチャ、公開キー トークンは含めないでください。</span><span class="sxs-lookup"><span data-stu-id="b0847-123">The friend assembly name that is passed to the <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> attribute cannot be the strong name of assembly *B*: do not include the assembly version, culture, architecture, or public key token.</span></span>

- <span data-ttu-id="b0847-124">アセンブリ *A* が厳密名でない場合、フレンド アセンブリの名前は、アセンブリ名のみで構成されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0847-124">If assembly *A* is not strong named, the friend assembly name should consist of only the assembly name.</span></span> <span data-ttu-id="b0847-125">詳細については、「[方法 :署名のないフレンド アセンブリを作成する (C#)](../../csharp/programming-guide/concepts/assemblies-gac/how-to-create-unsigned-friend-assemblies.md)」または「[方法: 署名のないフレンド アセンブリを作成する (Visual Basic)](../../visual-basic/programming-guide/concepts/assemblies-gac/how-to-create-unsigned-friend-assemblies.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b0847-125">For more information, see [How to: Create Unsigned Friend Assemblies (C#)](../../csharp/programming-guide/concepts/assemblies-gac/how-to-create-unsigned-friend-assemblies.md) or [How to: Create Unsigned Friend Assemblies (Visual Basic)](../../visual-basic/programming-guide/concepts/assemblies-gac/how-to-create-unsigned-friend-assemblies.md).</span></span>

- <span data-ttu-id="b0847-126">アセンブリ *B* に厳密な名前が付けられている場合、プロジェクトの設定またはコマンド ラインの `/keyfile` コンパイラ オプションを使用してアセンブリ *B* の厳密名キーを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0847-126">If assembly *B* is strong named, you must specify the strong-name key for assembly *B* by using the project setting or the command-line `/keyfile` compiler option.</span></span> <span data-ttu-id="b0847-127">詳細については、「[方法 :署名されたフレンド アセンブリを作成する (C#)](../../csharp/programming-guide/concepts/assemblies-gac/how-to-create-signed-friend-assemblies.md)」または「[方法: 署名されたフレンド アセンブリを作成する (Visual Basic)](../../visual-basic/programming-guide/concepts/assemblies-gac/how-to-create-signed-friend-assemblies.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b0847-127">For more information, see [How to: Create Signed Friend Assemblies (C#)](../../csharp/programming-guide/concepts/assemblies-gac/how-to-create-signed-friend-assemblies.md) or [How to: Create Signed Friend Assemblies (Visual Basic)](../../visual-basic/programming-guide/concepts/assemblies-gac/how-to-create-signed-friend-assemblies.md).</span></span>

 <span data-ttu-id="b0847-128"><xref:System.Security.Permissions.StrongNameIdentityPermission> クラスも型を共有する機能を提供しますが、次のような違いがあります。</span><span class="sxs-lookup"><span data-stu-id="b0847-128">The <xref:System.Security.Permissions.StrongNameIdentityPermission> class also provides the ability to share types, with the following differences:</span></span>

- <span data-ttu-id="b0847-129">フレンド アセンブリはアセンブリ全体に適用されますが、<xref:System.Security.Permissions.StrongNameIdentityPermission> は個々の型に適用されます。</span><span class="sxs-lookup"><span data-stu-id="b0847-129"><xref:System.Security.Permissions.StrongNameIdentityPermission> applies to an individual type, while a friend assembly applies to the whole assembly.</span></span>

- <span data-ttu-id="b0847-130">アセンブリ *B* との間で共有したい型がアセンブリ *A* に数百個存在する場合、そのすべてに対して <xref:System.Security.Permissions.StrongNameIdentityPermission> を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0847-130">If there are hundreds of types in assembly *A* that you want to share with assembly *B*, you have to add <xref:System.Security.Permissions.StrongNameIdentityPermission> to all of them.</span></span> <span data-ttu-id="b0847-131">フレンド アセンブリを使用した場合、フレンド関係を 1 回宣言するだけで済みます。</span><span class="sxs-lookup"><span data-stu-id="b0847-131">If you use a friend assembly, you only need to declare the friend relationship once.</span></span>

- <span data-ttu-id="b0847-132"><xref:System.Security.Permissions.StrongNameIdentityPermission> を使用する場合、共有する型をパブリックとして宣言する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0847-132">If you use <xref:System.Security.Permissions.StrongNameIdentityPermission>, the types you want to share have to be declared as public.</span></span> <span data-ttu-id="b0847-133">フレンド アセンブリを使用する場合、共有する型は `internal` (C# の場合、または Visual Basic では `Friend`) として宣言されます。</span><span class="sxs-lookup"><span data-stu-id="b0847-133">If you use a friend assembly, the shared types are declared as `internal` (in C# or `Friend` in Visual Basic).</span></span>

<span data-ttu-id="b0847-134">アセンブリの `internal` (C# の場合、または Visual Basic では `Friend`) な型とメソッドにモジュール ファイル (.netmodule 拡張子の付いたファイル) からアクセスする方法については、「[/moduleassemblyname (C#)](../../csharp/language-reference/compiler-options/moduleassemblyname-compiler-option.md)」または「[/moduleassemblyname (Visual Basic)](../../visual-basic/reference/command-line-compiler/moduleassemblyname.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b0847-134">For information about how to access an assembly's `internal` (in C# or `Friend` in Visual Basic) types and methods from a module file (a file with the .netmodule extension), see [/moduleassemblyname (C#)](../../csharp/language-reference/compiler-options/moduleassemblyname-compiler-option.md) or [/moduleassemblyname (Visual Basic)](../../visual-basic/reference/command-line-compiler/moduleassemblyname.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="b0847-135">関連項目</span><span class="sxs-lookup"><span data-stu-id="b0847-135">See also</span></span>

- <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute>
- <xref:System.Security.Permissions.StrongNameIdentityPermission>
- [<span data-ttu-id="b0847-136">方法: 署名のないフレンド アセンブリを作成する (C#)</span><span class="sxs-lookup"><span data-stu-id="b0847-136">How to: Create Unsigned Friend Assemblies (C#)</span></span>](../../csharp/programming-guide/concepts/assemblies-gac/how-to-create-unsigned-friend-assemblies.md)
- [<span data-ttu-id="b0847-137">方法: 署名のないフレンド アセンブリを作成する (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="b0847-137">How to: Create Unsigned Friend Assemblies (Visual Basic)</span></span>](../../visual-basic/programming-guide/concepts/assemblies-gac/how-to-create-unsigned-friend-assemblies.md)
- [<span data-ttu-id="b0847-138">方法: 署名されたフレンド アセンブリを作成する (C#)</span><span class="sxs-lookup"><span data-stu-id="b0847-138">How to: Create Signed Friend Assemblies (C#)</span></span>](../../csharp/programming-guide/concepts/assemblies-gac/how-to-create-signed-friend-assemblies.md)
- [<span data-ttu-id="b0847-139">方法: 署名されたフレンド アセンブリを作成する (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="b0847-139">How to: Create Signed Friend Assemblies (Visual Basic)</span></span>](../../visual-basic/programming-guide/concepts/assemblies-gac/how-to-create-signed-friend-assemblies.md)
- [<span data-ttu-id="b0847-140">.NET のアセンブリ</span><span class="sxs-lookup"><span data-stu-id="b0847-140">Assemblies in .NET</span></span>](index.md)
- [<span data-ttu-id="b0847-141">C# プログラミング ガイド</span><span class="sxs-lookup"><span data-stu-id="b0847-141">C# Programming Guide</span></span>](../../csharp/programming-guide/index.md)
- [<span data-ttu-id="b0847-142">プログラミングの概念</span><span class="sxs-lookup"><span data-stu-id="b0847-142">Programming Concepts</span></span>](../../visual-basic/programming-guide/concepts/index.md)